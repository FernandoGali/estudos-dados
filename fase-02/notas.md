# Fase 2: Excel e Power Query

Base de prática: `esmagadora_modelo.xlsx` (uma Tabela por aba) e os 12 apontamentos mensais de 2025.
Arquivos desta fase ficam em `fase-02/`.

---

## Tópico 1: Referências e Tabelas (05/11/2026)

Arquivo: `topico01_referencias.xlsx`

### Teoria

- **Referência relativa (`D3`):** anda junto com a fórmula. Arrastou para baixo, muda a linha; para o lado, muda a coluna.
- **Referência absoluta (`$C$5`):** não anda. Uso para uma constante única: meta, câmbio, preço.
- **Referência mista (`$C5` ou `D$3`):** o `$` trava só o que vem logo depois dele. `$C5` trava a coluna; `D$3` trava a linha. É o que permite uma fórmula só preencher uma matriz inteira.
- **F4:** alterna entre os quatro tipos enquanto edito a fórmula.
- **Tabela do Excel (Ctrl+Alt+T):** lista de registros com nome. Cresce sozinha quando entra dado novo, e uma fórmula escrita numa célula preenche a coluna inteira. É a origem ideal para tabela dinâmica, gráfico e Power Query.
- **Referência estruturada:** `[@coluna]` lê o valor da mesma linha; `tbProducao[coluna]` lê a coluna inteira. Colchete duplo quando há colchete por dentro: `=[@[soja_esmagada_t]]*meta_hexano`.
- **Célula nomeada:** nome dado pela Caixa de Nome (à esquerda da barra de fórmulas). Funciona como referência absoluta legível, válida em qualquer aba. Conferir e corrigir em Fórmulas > Gerenciador de Nomes.

### Prática

**Parte A: matriz de consumo permitido (aba `metas`)**

Plano de esmagamento de 2025 (linha 3, de D a O) × metas por indicador (coluna C). Uma fórmula em D5, arrastada até O7:

```
=D$3*$C5
```

- `D$3`: o plano fica na linha 3 e anda mês a mês.
- `$C5`: a meta fica na coluna C e desce indicador a indicador.

| Indicador | Meta | Jan | Total 2025 |
|---|---|---|---|
| Hexano | 1 L/t | 38.000 L | 496.000 L |
| Vapor | 0,27 t/t | 10.260 t | 133.920 t |
| Energia | 45 kWh/t | 1.710.000 kWh | 22.320.000 kWh |

Erros que cometi no caminho:
- A primeira fórmula, `=D3*$C$5`, só funcionava na linha do hexano. Arrastando para baixo, o plano caía numa linha vazia e a meta continuava a do hexano.
- Montei a matriz como Tabela. Matriz de cálculo funciona melhor como intervalo comum; Tabela é para lista de registros.

**Parte B: Tabela e referência estruturada (aba `fato_producao`)**

- Tabela renomeada para `tbProducao`.
- Célula `metas!C5` nomeada `meta_hexano`.
- Colunas novas:
  - `hexano_permitido_L` = `[@[soja_esmagada_t]]*meta_hexano`
  - `desvio_hexano_L` = `[@[hexano_l]]-[@[hexano_permitido_L]]`
- Conferência: 1.404,63 t × 1 L/t = 1.404,63 L permitidos; consumo de 1.805 L; desvio de +400,37 L.
- Teste de crescimento: digitei uma data na linha abaixo da Tabela, a Tabela se estendeu e as colunas novas se preencheram sozinhas.

Erros que cometi no caminho:
- Escrevi `meta_hexano` dentro da célula, no lugar do rótulo. O nome vai na Caixa de Nome.
- Faltou um colchete: `=[@[soja_esmagada_t]*meta_hexano` dá erro de sintaxe. Mais seguro digitar `=` e clicar na célula da mesma linha.

**Observação para o CP1:** com meta única de 1,0 L/t, todos os dias de 2022 aparecem acima da meta (400 a 600 L por dia), porque nessa época o índice ficava em 1,3 a 1,4 L/t. Meta precisa valer por período; senão o histórico antigo parece ruim o tempo todo.

### Perguntas

**1. O que acontece se eu escrever `=D3*C5`, sem `$`, em D5 e arrastar?**

As duas referências andam junto com a fórmula. Arrastando para o lado, E5 vira `=E3*D5`: o plano de fevereiro multiplicado pelo resultado de janeiro, 40.000 × 38.000 = 1,52 bilhão de litros. Arrastando para baixo, D6 vira `=D4*C6`; D4 está vazia e o resultado é zero. E6 vira `=E4*D6` e também dá zero.

**2. Por que `$C5` e não `$C$5`?**

A meta está sempre na coluna C, mas cada indicador tem a sua, na própria linha. Ao descer, a linha precisa mudar para pegar a meta do vapor e da energia. Com `$C$5`, as três linhas usariam a meta do hexano, e o vapor de janeiro sairia 38.000 em vez de 10.260.

**3. Duas vantagens da Tabela sobre um intervalo comum, pensando no boletim do CP1**

1. Ela cresce sozinha: quando entra o dia novo, as fórmulas, os gráficos e as tabelas dinâmicas do boletim já incluem esse dia, sem risco de alguém esquecer de estender o intervalo e o total sair sem os últimos dias.
2. A fórmula fica legível: `tbProducao[soja_esmagada_t]` mostra o que está sendo calculado, enquanto `C2:C1735` não diz nada. Isso facilita conferir e passar a planilha para outra pessoa.

### Pendências para o CP1

- [ ] Renomear as Tabelas da base (Tabela2 a Tabela12) com nomes claros: `tbParadas`, `tbRecebimento`, `dimFornecedor` e assim por diante.
- [ ] Renomear as colunas `indice` para `indice_vapor_kg_t` e `indice_hexano_l_t`.
- [ ] Definir meta por período (antes e depois de 2023) em vez de meta única.

---

## Tópico 2: SE, SES, E, OU e SEERRO (05/11/2026)

Arquivo único da fase: `fase02_producao.xlsx` (antigo `topico01_referencias.xlsx`). A `tbProducao` acumula as colunas de cada tópico; a conferência deste tópico fica na aba `t02_conferencia`.

### Teoria

- **SE:** um teste, dois resultados. No Excel em português, os argumentos são separados por `;`.
- **SE aninhado:** um SE dentro do outro, para três ou mais resultados. Vira uma escada de parênteses.
- **SES:** lista de pares teste e resultado. Sempre terminar com `VERDADEIRO; "..."`, que funciona como "senão"; sem ele, um valor que não passa em nenhum teste dá `#N/D`.
- **A ordem dos testes importa:** o Excel para no primeiro teste verdadeiro. Ir do caso mais restrito para o mais amplo.
- **E / OU:** entram no lugar do teste do SE e juntam várias condições numa só. `E` exige todas; `OU`, pelo menos uma.
- **SEERRO:** troca qualquer erro por outro valor. Esconde também os erros que eu não esperava.

### Prática (colunas novas na `tbProducao`)

| Coluna | Fórmula | O que faz |
|---|---|---|
| `status_dia` | `=SES([@[soja_esmagada_t]]=0;"Parado";[@[soja_esmagada_t]]<1000;"Carga Reduzida";VERDADEIRO;"Carga Normal")` | classifica o dia |
| `status_dia_se` | `=SE([@[soja_esmagada_t]]=0;"Parado";SE([@[soja_esmagada_t]]<1000;"Carga Reduzida";"Carga Normal"))` | a mesma regra com SE aninhado |
| `dia_parcial` | `=SES([@[soja_esmagada_t]]=0;"parado";E([@[soja_esmagada_t]]>0;[@[horas_trabalhadas]]<20);"partida/parada";E([@[soja_esmagada_t]]>0;[@[horas_trabalhadas]]<24);"parada curta";VERDADEIRO;"dia cheio")` | separa partida/parada de parada curta |
| `alerta_de_consumo` | `=SE(OU([@[indice_hexano_l]]>1,5;[@[indice_vapor_kg]]>300);"verificar";"")` | alerta de consumo fora do normal |
| `indice_hexano_seerro` | `=SEERRO([@[hexano_l]]/[@[soja_esmagada_t]];"")` | índice sem `#DIV/0!` (esconde qualquer erro) |
| `indice_hexano_se` | `=SE([@[soja_esmagada_t]]=0;"";[@[hexano_l]]/[@[soja_esmagada_t]])` | índice sem `#DIV/0!` (trata só o dia parado) |

**Comparar duas colunas sem coluna auxiliar:**

```
=SOMA(--(tbProducao[status_dia]<>tbProducao[status_dia_se]))
```

`<>` compara linha a linha e gera VERDADEIRO/FALSO; `--` converte em 1/0; `SOMA` conta as diferenças. Resultado 0 = colunas idênticas. Para ver onde estão as diferenças: formatação condicional com `=$P2<>$Q2`.

**Conferência (aba `t02_conferencia`):**

| Status | Dias | % |
|---|---|---|
| Parado | 355 | 20% |
| Carga Reduzida | 89 | 5% |
| Carga Normal | 1.290 | 74% |
| **Soma** | **1.734** | = `LINS(tbProducao)` |
| Dias com alerta de consumo | 174 | 12,6% dos 1.379 dias de operação |

Leitura: a fábrica opera em campanha. Um em cada cinco dias está parado e quase não existe "rodar devagar"; a carga reduzida é praticamente só partida e parada.

Erros que cometi no caminho:
- Último teste do SES como `> 1000` em vez de `VERDADEIRO`: um dia com exatamente 1.000 t daria `#N/D`.
- Coloquei o `OU` no lugar do "senão" do SE, e não no lugar do teste. O vapor nunca disparava o alerta sozinho.
- Usei a meta como limite de alerta: o alerta disparou em 1.057 dias (77% da operação). Meta e alerta são coisas diferentes.
- Montei a conferência uma coluna à direita e as fórmulas apontavam para a coluna vazia: tudo deu 0. Quando tudo dá zero, a primeira suspeita é a referência.

### Aprendizados

- **Se o dado existe na fonte, uso a fonte; só infiro quando não há registro.** Partida e parada de fábrica são anunciadas e ficam no registro de paradas (`fato_paradas`). Deduzir pelas horas trabalhadas é adivinhar o que a empresa já sabe. Classificar pela duração só faz sentido para paradas não programadas.
- **Meta não é limite de alerta.** A meta responde "estamos bons?" e é ultrapassada com frequência; o alerta responde "alguém precisa olhar isso hoje?" e deve disparar só na exceção. Alerta que dispara todo dia vira ruído (fadiga de alarme). Na Fase 8 (CEP), os limites vão sair de média e desvio padrão.
- **Antes de entregar uma regra, conto quantas vezes ela dispara.** Olhando as primeiras linhas, o alerta parecia certo; só a contagem mostrou que não servia.

### Perguntas

**1. Quando usar SES em vez de SE aninhado?**

O resultado é o mesmo. Com três ou mais faixas, uso `SES`, porque é uma lista linear, mais fácil de ler e de manter, sempre com `VERDADEIRO` no fim como "senão". Uso `SE` aninhado quando a planilha precisa rodar em Excel anterior a 2019, que não tem `SES`, ou quando a lógica tem ramos, e não uma lista de faixas.

**2. Por que a ordem dos testes importa?**

O Excel para no primeiro teste verdadeiro. Se eu testar "< 1.000" antes de "= 0", um dia parado, com 0 t, passa no primeiro teste e vira "Carga Reduzida". Na minha base, os 355 dias parados sumiriam e a carga reduzida saltaria de 89 para 444 dias. Por isso vou do caso mais restrito para o mais amplo: primeiro o zero, depois menor que 1.000, e o resto é normal.

**3. Por que o SEERRO pode ser perigoso num boletim de produção?**

O `SEERRO` esconde qualquer erro, não só o que eu esperava. No teste, digitei um texto no lugar do esmagado e o `SEERRO` deixou o dia vazio, como se fosse uma parada; no boletim, um erro de lançamento viraria um dia parado sem ninguém perceber. Prefiro o `SE`, que trata só o caso esperado (esmagado = 0) e deixa os erros inesperados aparecerem como `#VALOR!`. E devolvo vazio, não zero: o zero entraria na média do mês e distorceria o indicador. Uso `SEERRO` só quando sei exatamente qual erro vai acontecer e que ele é normal.

### Pendências para o CP1

- [ ] Trazer o tipo da parada (programada ou não) da `fato_paradas` em vez de deduzir pelas horas (Tópico 3, PROCX).
- [ ] Confirmar onde se concentram os 174 alertas: dias de partida/parada? 2022 e 2023, com índice perto de 1,5? (Tópico 11, tabela dinâmica)
- [ ] Definir os limites de alerta por período, como a meta.
