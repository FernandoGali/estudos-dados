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

---

## Tópico 3: PROCV, PROCX e ÍNDICE+CORRESP (06/11/2026)

Tabelas renomeadas: `tbParadas`, `dimMotivo`, `dimEquipamento`, `tbRecebimento`, `dimFornecedor`. Abas novas: `t03_procura` e `t03_faixas`.

### Teoria

- **PROCV:** `=PROCV(valor;tabela;nº da coluna;FALSO)`. Procura só na 1ª coluna, devolve só para a direita, aponta a coluna pelo número e é aproximado por padrão (sem o `FALSO`, devolve um valor parecido em vez de erro).
- **PROCX:** `=PROCX(valor;coluna onde procurar;coluna a devolver;[se não encontrar];[modo])`. Procura para qualquer lado, aponta a coluna pelo nome, é exato por padrão e tem o 4º argumento para o "não encontrado".
- **ÍNDICE + CORRESP:** `=ÍNDICE(coluna a devolver;CORRESP(valor;coluna onde procurar;0))`. O CORRESP diz a posição; o ÍNDICE pega o que está nessa posição. Funciona em qualquer versão do Excel. Sem o `0` no CORRESP, a busca é aproximada e pode devolver posição errada sem aviso.
- **Exata ou aproximada:** exata para códigos e IDs; aproximada para faixas. No PROCX, modo `-1` = "exato ou o próximo menor".
- **Referência estruturada funciona de qualquer aba:** `dimEquipamento[tag]` lê a coluna da Tabela onde ela estiver.
- **O que o PROCX devolve:** `#N/D` quando não encontra; `0` quando encontra, mas a célula de retorno está vazia.

### Prática

| Exercício | Fórmula | Conferência |
|---|---|---|
| Tipo da parada (PROCV) | `=PROCV([@id_motivo];dimMotivo;3;FALSO)` | 465 planejadas, 605 não planejadas |
| Tipo da parada (PROCX) | `=PROCX([@id_motivo];dimMotivo[id_motivo];dimMotivo[tipo])` | igual ao PROCV (0 diferenças) |
| Tipo da parada (ÍNDICE) | `=ÍNDICE(dimMotivo[tipo];CORRESP([@id_motivo];dimMotivo[id_motivo];0))` | igual ao PROCX (0 diferenças) |
| Tag do equipamento | `=PROCX([@id_equipamento];dimEquipamento[id_equipamento];dimEquipamento[tag];"sem equipamento")` | 382 paradas sem equipamento |
| Procura para a esquerda (tag → id) | `=ÍNDICE(dimEquipamento[id_equipamento];CORRESP(B2;dimEquipamento[tag];0))` | RSC-01 = 15; PROCV dá `#N/D` |
| Região do fornecedor | `=PROCX([@id_fornecedor];dimFornecedor[id_fornecedor];dimFornecedor[regiao];"sem fornecedor")` | 0 cargas sem fornecedor |
| Faixa de umidade | `=PROCX([@umidade_pct];tbFaixaUmidade[limite_inferior];tbFaixaUmidade[faixa];"sem umidade";-1)` | 46.406 / 3.977 / 351 / 9 = 50.743 |
| Houve parada planejada no dia? | `=SE(CONT.SES(tbParadas[data];[@data];tbParadas[tipo_procx];"Planejada")>0;"sim";"não")` | 455 dias |

**A armadilha do PROCV (T3-P2):** inseri uma coluna entre `motivo` e `tipo` na `dimMotivo`. O PROCV passou a mostrar 0 nas 1.070 paradas, sem nenhum erro aparente; PROCX e ÍNDICE continuaram certos.

**Leitura da umidade:** 91,4% das cargas chegam até 14% (a referência de comercialização); 8,6% (4.337 cargas) chegam acima e geram desconto; 9 cargas acima de 18%. A regra das faixas fica numa Tabela, e não dentro da fórmula: se a política mudar, muda a tabela e as contagens se refazem sozinhas.

Erros que cometi no caminho:
- Comparei três colunas de uma vez (`A<>B<>C`). O Excel compara da esquerda para a direita: `A<>B` vira FALSO, e `FALSO<>"texto"` é sempre VERDADEIRO. Resultado: 1.070 "diferenças". Comparar duas por vez.
- No ÍNDICE+CORRESP, usei a célula da tag como coluna de retorno, procurei na coluna errada e esqueci o `0` do CORRESP.
- Escrevi a contagem final dentro da Tabela e ela virou uma coluna com o mesmo número em todas as linhas. Conferência fica fora da Tabela.

### Aprendizados

- **Dado ausente nem sempre é dado faltando.** As 382 paradas sem equipamento são falta de matéria-prima, entressafra, preventiva e energia externa: motivos que não dependem de máquina. O vazio é esperado; seria falha se aparecesse uma "quebra de eixo" sem equipamento.
- **Separar base, cálculo e apresentação.** A aba de recebimento tinha um bloco de total e média escrito embaixo dos dados, fora da Tabela. Isso trava o crescimento da Tabela e confunde quem lê (e confundiu a conferência). Resumo fica em outra aba, calculado a partir da Tabela.
- **Granularidade:** é o que cada linha representa. `tbParadas` = uma linha por parada; `tbProducao` = uma linha por dia. PROCX serve para muitos para um (parada → motivo). Quando um dia tem várias paradas, a pergunta certa é "houve?", respondida contando.

### Perguntas

**1. Por que evitar PROCV numa planilha que outras pessoas vão manter?**

O PROCV aponta para a coluna pelo número, e não pelo nome. Se alguém insere ou apaga uma coluna na tabela de cadastro, ele passa a buscar outra coluna sem dar erro. No meu teste, inseri uma coluna e as 1.070 paradas passaram a mostrar 0, sem nenhum aviso. O PROCX aponta pelo nome da coluna e continua certo. Por isso, em planilha que outras pessoas mantêm, uso PROCX.

**2. Quando o PROCX não encontra o valor, o que acontece? Por que o 4º argumento é melhor que o SEERRO?**

Quando não encontra, o PROCX devolve `#N/D`. Com o 4º argumento, eu defino o que aparece nesse caso, como "sem equipamento" nas 382 paradas que não dependem de máquina. A vantagem sobre o `SEERRO` é que ele trata só o caso de não encontrar: se aparecer outro erro, como um dado digitado errado, ele continua visível para alguém corrigir. O `SEERRO` esconderia tudo do mesmo jeito.

**3. Por que o PROCX pela data deu resposta errada? O que é granularidade?**

Granularidade é o que cada linha de uma tabela representa. Na `tbParadas`, cada linha é uma parada; na `tbProducao`, cada linha é um dia. Quando um dia tem mais de uma parada, o PROCX pela data devolve só a primeira que encontra. No dia 05/11/2022, ele mostrou "Não planejada", mas o dia também teve uma parada planejada: a resposta não está errada, está incompleta. Isso acontece em 87 dias. A pergunta certa nesse caso é "houve parada planejada?", e ela se responde contando com `CONT.SES`.

### Pendências para o CP1

- [ ] Ver se as cargas acima de 14% de umidade se concentram em algum fornecedor ou região (Tópico 11, tabela dinâmica).
- [ ] No boletim, usar "houve parada planejada?" (contagem) e não o PROCX pela data.
- [ ] Apagar o bloco de resumo que estava embaixo dos dados de recebimento.

---

## Tópico 4: Funções de data (06/11/2026)

Tabela renomeada: `dimCalendario`. Aba nova: `t04_datas`.

### Teoria

- **Data é número:** o Excel guarda cada data como a quantidade de dias desde 1900 (01/01/2022 = 44.562). O formato é só a aparência. Por isso dá para subtrair (dias entre duas datas) e somar (data + 7 = uma semana depois). De 01/01 a 10/01, a subtração dá 9; para contar os dois dias, somar 1.
- **ANO, MÊS, DIA:** extraem as partes da data.
- **NÚMSEMANA(data):** semana começa no domingo; semana 1 é a do dia 1º de janeiro.
- **NÚMSEMANA(data;21):** padrão ISO; semana começa na segunda; semana 1 é a da primeira quinta-feira do ano. Toda semana ISO tem 7 dias completos.
- **FIMMÊS(data;0):** último dia do mês. `DIA(FIMMÊS(data;0))` = dias no mês.
- **DIATRABALHO(início;dias;[feriados]):** data depois de N dias úteis. Pula sábado, domingo e os feriados da lista. Começa a contar no dia seguinte ao início.
- **HOJE():** muda sozinho a cada abertura do arquivo. Não usar em base histórica nem em boletim de dia escolhido.

### Prática (colunas novas na `tbProducao`)

| Coluna | Fórmula |
|---|---|
| `ano` | `=ANO([@data])` |
| `mes` | `=MÊS([@data])` |
| `semana_excel` | `=NÚMSEMANA([@data])` |
| `semana_iso` | `=NÚMSEMANA([@data];21)` |
| `dias_no_mes` | `=DIA(FIMMÊS([@data];0))` |
| `semana_iso_cal` | PROCX da `semana_iso` da `dimCalendario` pela data |
| `periodo` | `=SE(E([@mes]>=2;[@mes]<=7);"Safra";"Entressafra")` |
| `periodo_cal` | PROCX do `periodo` da `dimCalendario` pela data |
| `ultima_falha_mec` | `=MÁXIMOSES(tbParadas[data];tbParadas[id_motivo];2;tbParadas[data];"<="&[@data])` |
| `dias_sem_falha_mec` | `=SE([@ultima_falha_mec]=0;"sem histórico";[@data]-[@ultima_falha_mec])` |
| `dias_operacao_sem_falha` | `=CONT.SES(tbProducao[data];">"&[@ultima_falha_mec];tbProducao[data];"<="&[@data];tbProducao[status_dia];"<>Parado")` |

Conferências:
- 01/01/2022 (sábado): semana Excel 1, semana ISO 52. 03/01/2022 (segunda): ISO 1.
- `semana_iso` × `dimCalendario`: 0 diferenças. `periodo` × `dimCalendario`: 0 diferenças.
- Safra (fevereiro a julho): 906 dias. Entressafra: 828 dias.
- Dias sem falha mecânica: 01/01/2022 sem histórico; 10/01/2022 = 7; 15/06/2024 = 0; 30/09/2026 = 16. Maior intervalo: 66 dias.
- Dias úteis: pedido em 20/11/2026 com 10 dias úteis = entrega em 04/12/2026. Os feriados da lista (20/11 e 25/12) não mudaram o prazo: o 20/11 é o próprio dia do pedido e o 25/12 fica depois da entrega. Feriado só muda o prazo se cair dentro do período.

**O indicador que mentia:**

| Dia | Dias corridos sem falha | Dias de operação sem falha |
|---|---|---|
| 22/01/2025 | 66 (o "recorde") | 16 |
| 30/09/2026 | 16 | 5 |

Os maiores intervalos de dias corridos caem todos na entressafra de 2024/2025, com a fábrica parada boa parte do tempo. Fábrica parada não quebra. O maior intervalo de operação real sem falha mecânica foi de 30 dias, em 21/02/2023, no meio da safra.

Erros que cometi no caminho:
- A coluna nova herdou o formato de data da vizinha, e o ano 2022 apareceu como 14/07/1905. O cálculo estava certo; era só o formato.
- Nos primeiros dias da base, sem falha anterior, o MÁXIMOSES devolveu 0 e a subtração virou a própria data em número (44.562 dias).
- Montei o SE com a subtração no lugar do teste. O teste tem de ser uma pergunta de sim ou não: `[@ultima_falha_mec]=0`.
- A lista de feriados tinha uma linha vazia no meio, e o intervalo da fórmula deixava o Natal de fora.
- Tentei o CONT.SES fora da Tabela e na linha do cabeçalho: `[@...]` só funciona numa linha de dados da Tabela.

### Aprendizados

- **Indicador de calendário em fábrica que opera em campanha engana.** "Dias sem falha" e "dias sem acidente" contam o calendário, e não a exposição ao risco. Se a máquina não girou, ela não quebrou. Fábrica parada não é motivo para comemorar número de manutenção ou de segurança: fábrica parada é caixa parado. A métrica formal de manutenção é o MTBF, em horas trabalhadas.
- **Quando calculo algo que já existe na fonte, confiro um contra o outro.** A semana ISO e o período de safra bateram com a `dimCalendario` em todos os dias.

### Perguntas

**1. Por que dá para subtrair uma data de outra no Excel?**

O Excel guarda cada data como um número, a quantidade de dias desde 1900: 01/01/2022 é o 44.562. Por isso dá para subtrair, e o resultado é o número de dias entre as duas datas. De 01/01 a 10/01, dá 9; se eu quiser contar os dois dias, somo 1. Foi assim que calculei os dias sem falha mecânica.

**2. Por que a semana de 01/01/2022 deu 1 numa fórmula e 52 na outra? Qual usar?**

A semana do Excel começa no domingo, e a semana 1 é a do dia 1º de janeiro. A ISO começa na segunda, e a semana 1 é a da primeira quinta-feira do ano. Por isso o 01/01/2022, um sábado, ficou na semana 1 numa e na 52 de 2021 na outra. Num relatório de produção, uso a ISO: toda semana tem 7 dias completos, então as semanas são comparáveis entre si. Na contagem do Excel, a semana 1 de 2022 teve um dia só. Mas o mais importante é seguir o padrão da empresa e deixar escrito no relatório qual é.

**3. Por que não usar HOJE() numa base histórica ou num boletim de dia escolhido?**

O `HOJE()` se atualiza toda vez que o arquivo abre. Se o boletim do dia 05/11 usa `HOJE()`, ao ser aberto no dia 06 ele passa a mostrar o dia 06. Não dá para refazer o boletim de um dia passado, e um cálculo como "dias sem falha" muda de valor sem ninguém mexer. Por isso, no boletim, a data fica numa célula que a pessoa escolhe, e todas as fórmulas leem dessa célula.

### Pendências para o CP1

- [ ] Esmagamento por semana ISO e por mês (Tópico 9, SOMASES).
- [ ] No boletim, usar dias de operação sem falha (ou MTBF em horas trabalhadas), e não dias corridos.
- [ ] Data do boletim numa célula de entrada, nunca `HOJE()`.
- [ ] Lista de feriados como Tabela (`tbFeriados`), para crescer sozinha.

---

## Tópico 5: SUBSTITUIR, ESQUERDA, DIREITA e companhia (09/11/2026)

Tudo na `tbParadas`. Objetivo: separar a tag do equipamento do texto da descrição ("Quebra de eixo RSC-01").

### Teoria

| Função | O que faz | Exemplo com "Quebra de eixo RSC-01" |
|---|---|---|
| `NÚM.CARACT(texto)` | quantos caracteres o texto tem | 21 |
| `ESQUERDA(texto;n)` | os n primeiros caracteres | `ESQUERDA(A2;14)` = "Quebra de eixo" |
| `DIREITA(texto;n)` | os n últimos caracteres | `DIREITA(A2;6)` = "RSC-01" |
| `EXT.TEXTO(texto;início;n)` | n caracteres a partir de uma posição | |
| `LOCALIZAR(procurado;texto)` | posição do trecho (ignora maiúsculas); `#VALOR!` se não achar | `LOCALIZAR("-";A2)` = 19 |
| `SUBSTITUIR(texto;antigo;novo;[nº])` | troca pelo conteúdo; o 4º argumento escolhe a ocorrência | `SUBSTITUIR(A2;"-";"")` |
| `MUDAR(texto;início;n;novo)` | troca pela posição | `MUDAR(A2;19;1;"")` |
| `TEXTODEPOIS(texto;delim;[nº])` | o que vem depois do delimitador; `-1` = o último | `TEXTODEPOIS(A2;" ";-1)` = "RSC-01" |
| `TEXTOANTES(texto;delim;[nº])` | o que vem antes do delimitador | `TEXTOANTES(A2;" ";-1)` = "Quebra de eixo" |

- **"Contém?" no Excel:** `ÉNÚM(LOCALIZAR("-";texto))`. O LOCALIZAR devolve uma posição ou um erro; o ÉNÚM transforma em VERDADEIRO ou FALSO para o SE.
- **`""` não é `" "`:** vazio é célula sem nada; espaço é um caractere invisível que conta como preenchido.
- **Operadores de comparação:** `=` igual, `<>` diferente, `>` maior, `<` menor, `>=` maior ou igual, `<=` menor ou igual. Numa fórmula de lista, `*` funciona como E.
- **Moldes:** palavras em minúsculas com sublinhado (`quantos_caracteres`) são lacunas a preencher, não texto para copiar.

### Prática (colunas novas na `tbParadas`)

| Coluna | Fórmula | Resultado |
|---|---|---|
| `tag_direita` | `=DIREITA([@descricao];6)` | falha: " DT-01" (6 caracteres, com espaço) e " de MP" |
| `ultima_palavra` | `=TEXTODEPOIS([@descricao];" ";-1)` | "DT-01" (5 caracteres), "MP" |
| `tag_texto` | `=SE(ÉNÚM(LOCALIZAR("-";[@ultima_palavra]));[@ultima_palavra];"")` | 570 tags |
| `descricao_limpa` | `=SE([@[tag_texto]]="";[@descricao];ESQUERDA([@descricao];NÚM.CARACT([@descricao])-NÚM.CARACT([@[tag_texto]])-1))` | "Quebra de eixo" |
| `ultima_palavra_classica` | `=DIREITA([@descricao];NÚM.CARACT([@descricao])-LOCALIZAR("#";SUBSTITUIR([@descricao];" ";"#";NÚM.CARACT([@descricao])-NÚM.CARACT(SUBSTITUIR([@descricao];" ";"")))))` | igual à `ultima_palavra` |
| `tag_sem_hifen_sub` | `=SUBSTITUIR([@tag_texto];"-";"")` | "RSC01" |
| `tag_sem_hifen_mudar` | `=SE([@[tag_texto]]="";"";MUDAR([@[tag_texto]];LOCALIZAR("-";[@[tag_texto]]);1;""))` | igual à de cima |

**A fórmula clássica do último espaço, passo a passo** ("Quebra de eixo RSC-01", 21 caracteres):
1. Texto sem espaços: 18 caracteres. 21 − 18 = 3 espaços.
2. `SUBSTITUIR(texto;" ";"#";3)` troca só o 3º espaço, o último: "Quebra de eixo#RSC-01".
3. `LOCALIZAR("#";...)` = 15, a posição do último espaço.
4. `DIREITA(texto;21−15)` = "RSC-01".

Mais simples no Excel 365: `TEXTODEPOIS` e `TEXTOANTES`. A clássica serve para Excel antigo.

**Conferência com a fonte:**

| Verificação | Resultado |
|---|---|
| Paradas com tag na descrição | 570 |
| Paradas sem tag na descrição | 500 |
| Diferenças entre a tag do texto e a do cadastro, onde há tag no texto | 0 |
| Paradas com equipamento no cadastro e sem tag no texto | 118 |

As 118 são armazém de farelo cheio (83), queda de energia da concessionária (24, registrada na subestação SUB-01) e inspeção de segurança (11, no extrator EXT-01). A causa não é o equipamento, mas o cadastro registra onde a parada aconteceu. Usando só a descrição, essas 118 paradas ficariam sem equipamento.

Erros que cometi no caminho:
- Copiei o molde com as lacunas (`TEXTO`, `quantos_caracteres`) e deu `#VALOR!`. Separei a conta numa coluna temporária, achei o 14 e encaixei no lugar da lacuna.
- No MUDAR, fechei o parêntese do LOCALIZAR depois do `;1`: o 1 virou "posição inicial da busca" e o MUDAR ficou só com 2 dos 4 argumentos.

### Aprendizados

- **Extrair por tamanho fixo quebra.** Tag de 5 caracteres sai com espaço invisível; descrição sem tag vira lixo. Cortar por delimitador (último espaço) e validar o resultado (tem hífen?).
- **Para achar onde uma fórmula longa quebra, separo as partes em colunas temporárias.**
- **Campo estruturado é a fonte; texto livre é a conferência.** Onde os dois discordam, um está errado e precisa ser investigado.

### Perguntas

**1. Por que `DIREITA(descricao;6)` não serve para extrair a tag?**

O `DIREITA(descricao;6)` supõe que toda tag tem 6 caracteres e que toda descrição termina com uma tag. As duas suposições falham. A tag DT-01 tem 5 caracteres, então ele pegou " DT-01" com um espaço na frente: na tela parece igual, mas tem 6 caracteres e não acharia o equipamento num PROCX. E "Falta de MP" não tem tag, então ele devolveu " de MP", um pedaço de texto sem sentido. O certo é cortar no último espaço, com `TEXTODEPOIS`, e só aceitar como tag a palavra que tem hífen.

**2. Qual a diferença entre SUBSTITUIR e MUDAR?**

O `SUBSTITUIR` troca pelo conteúdo: acha o hífen onde ele estiver, e troca todos, ou só uma ocorrência, com o 4º argumento. O `MUDAR` troca pela posição: precisa saber em que letra começar e quantas trocar. Para tirar o hífen da tag, uso o `SUBSTITUIR`, que é mais simples. Em "RSC-01-A", ele tira os dois hífens, e o `MUDAR` com `LOCALIZAR` tiraria só o primeiro. Uso o `MUDAR` quando a posição é a regra, como mascarar os caracteres 4 a 6 de um código.

**3. Descrição ou cadastro: qual usar como fonte no boletim?**

Uso o cadastro, o `id_equipamento`, como fonte, porque é um campo estruturado: só aceita um código que existe na lista de equipamentos. A descrição é texto livre, e cada pessoa escreve de um jeito. Nas 118 paradas de armazém cheio, queda de energia e inspeção, o texto nem cita o equipamento. Mas não jogo a descrição fora: uso como conferência. Cruzei as duas e elas concordaram nas 570 paradas em que o texto tem tag. Numa base nova, eu faria esse cruzamento antes de escolher a fonte. Onde as duas discordam, é sinal de que uma está errada e precisa ser investigada.

### Pendências para o CP1

- [ ] No boletim, equipamento da parada sempre pelo `id_equipamento` (cadastro), nunca pela descrição.
- [ ] Apagar as colunas intermediárias de estudo (`tag_direita`, `ultima_palavra_classica`, `tag_sem_hifen_mudar`) quando montar o arquivo do CP1.

---

## Tópico 6: MAIÚSCULA, MINÚSCULA e PRI.MAIÚSCULA (10/11/2026)

Tudo na aba `t06_fornecedores`, Table `tbAmostraFornecedor`: uma amostra de 6.943 cargas de 5 fornecedores (5 `cod_fornecedor`), com o nome digitado de 37 jeitos diferentes. Objetivo: ver quanto as funções de texto conseguem padronizar e onde elas param.

### Teoria

| Função | O que faz | Exemplo com "grupo são pedro i" |
|---|---|---|
| `MAIÚSCULA(texto)` | tudo em maiúscula | "GRUPO SÃO PEDRO I" |
| `MINÚSCULA(texto)` | tudo em minúscula | "grupo são pedro i" |
| `PRI.MAIÚSCULA(texto)` | primeira letra de cada palavra em maiúscula | "Grupo São Pedro I" |
| `EXATO(texto1;texto2)` | compara diferenciando maiúsculas | `EXATO("ABC";"abc")` = FALSO |
| `ARRUMAR(texto)` | tira espaços do começo, do fim e duplos | " Faz.  Santa Luzia " → "Faz. Santa Luzia" |

- **O Excel não diferencia maiúsculas:** `="ABC"="abc"` dá VERDADEIRO, e o mesmo vale para PROCX, CONT.SE, ÚNICO e tabela dinâmica. Para diferenciar, uso `EXATO`.
- **Power Query, SQL (PostgreSQL) e pandas diferenciam.** O que o Excel agrupa em uma linha vira várias nessas ferramentas.
- **PRI.MAIÚSCULA não conhece sigla nem número romano:** "GRUPO HORIZONTE II" vira "Grupo Horizonte Ii". Preposições também viram "De", "Da", "Do".
- **Contar distintos:** `=CONT.VALORES(ÚNICO(coluna))`.

### Prática (colunas novas na `tbAmostraFornecedor`)

| Coluna | Fórmula | Nomes distintos |
|---|---|---|
| `fornecedor` (original) | | 24 no Excel (37 diferenciando maiúsculas) |
| `maiuscula` | `=MAIÚSCULA([@fornecedor])` | 24 |
| `minuscula` | `=MINÚSCULA([@fornecedor])` | 24 |
| `pri_maiuscula` | `=PRI.MAIÚSCULA([@fornecedor])` | 24 |
| `arrumado` | `=ARRUMAR(MAIÚSCULA([@fornecedor]))` | 10 |
| `padronizado` | `=SUBSTITUIR(SUBSTITUIR(SUBSTITUIR([@arrumado];"COOP. ";"COOPERATIVA ");"FAZ. ";"FAZENDA ");"AGRICOLA";"AGRÍCOLA")` | 6 |
| `final` | `=SUBSTITUIR([@padronizado];"SAO";"SÃO")` | 5 |

O que cada etapa resolveu:
- **Maiúsculas (24 → 24):** nada, porque o ÚNICO já ignorava a caixa das letras. Para o Excel, o problema nunca foi esse.
- **Espaços (24 → 10):** " Fazenda Santa Luzia" e "Fazenda Santa Luzia" são nomes diferentes até para o Excel.
- **Abreviação e acento (10 → 6 → 5):** "Coop.", "Faz.", "Agricola" e "Sao" só saíram com SUBSTITUIR, um caso de cada vez.

**Conferência com o código:**

| Verificação | Fórmula | Resultado |
|---|---|---|
| Códigos distintos | `=CONT.VALORES(ÚNICO(tbAmostraFornecedor[cod_fornecedor]))` | 5 |
| Combinações código + nome | `=CONT.VALORES(ÚNICO(tbAmostraFornecedor[cod_fornecedor]&"\|"&tbAmostraFornecedor[final]))` | 5 |

Combinação igual a 5 quer dizer que cada código tem um nome só e cada nome tem um código só. Sem esse teste, 5 nomes não provariam que cada carga ficou com o fornecedor certo.

Erro que cometi no caminho:
- Testei a combinação com o intervalo `[cod_fornecedor]:[final]` e deu 192. O intervalo pegou as 8 colunas entre as duas, e o ÚNICO devolveu as 24 combinações da coluna original. Como o CONT.VALORES conta células, e não linhas, deu 24 × 8 = 192. Para combinar só duas colunas, colo com `&`.

### Aprendizados

- **Padronizar texto ajuda, mas não resolve sozinho.** MAIÚSCULA e ARRUMAR resolvem o que é geral; abreviação e acento precisam de regra caso a caso, e cada variação nova pede um SUBSTITUIR novo.
- **A chave é o código.** O nome que vai para o relatório vem do cadastro (`dimFornecedor`), escrito certo uma vez, puxado pelo `cod_fornecedor`.
- **Com várias colunas, CONT.VALORES(ÚNICO()) conta células.** Para contar combinações, junto as colunas com `&` e um separador que não aparece nos dados.
- **Com muitos casos, fórmula vira bagunça.** O jeito melhor é uma tabela de-para ("como está" → "como deveria ser"), que vou fazer no Power Query.

### Perguntas

**1. O Excel contou menos nomes do que existem. Quando isso engana, quando ajuda, e o que acontece no Power Query ou no SQL?**

O Excel contou 24 nomes, e na verdade são 5 fornecedores. Se eu fizer um ranking de fornecedores pelo nome digitado, o mesmo fornecedor aparece dividido em vários nomes, cada um com só uma parte das cargas, e cai no ranking. O indicador fica errado sem dar erro nenhum. Por outro lado, o Excel ignorar maiúsculas ajuda no dia a dia: um PROCX acha "GRUPO DUAS PONTES" mesmo que o cadastro esteja como "Grupo Duas Pontes". No Power Query e no SQL é pior, porque eles diferenciam maiúsculas: a mesma coluna viraria 37 nomes em vez de 24. Uma análise que bate no Excel pode não bater quando eu levar para outra ferramenta.

**2. A coluna `final` bateu com o código. Por que não usar ela como nome oficial?**

Porque ela só corrige os casos que eu vi nesta amostra. Se amanhã alguém digitar "Faz Santa Luzia", sem o ponto, ou "Fazenda Sta Luzia", a fórmula não pega, e o fornecedor volta a se dividir. O nome também saiu todo em maiúscula, e a PRI.MAIÚSCULA estragaria o "II". O nome oficial vem da `dimFornecedor`, o cadastro de fornecedores, puxado pelo `cod_fornecedor` com um PROCX. O código é preenchido por seleção, não por digitação, então não varia. A coluna `final` serve para conferir, não para ser a fonte.

**3. Já viu o mesmo item escrito de vários jeitos no trabalho? Como aparecia?**

Nos relatórios não tínhamos muito dado no nível de equipamento, mas no sistema era comum. Quando eu buscava um item, era difícil saber qual era o certo, porque o mesmo item aparecia escrito de jeitos diferentes, às vezes com códigos diferentes. Isso gerava confusão e podia acabar na compra da peça errada. Hoje eu vejo que é o mesmo problema desta amostra: quando não existe um cadastro único, cada pessoa escreve de um jeito, e a busca, o relatório e a compra dependem de alguém adivinhar qual é o registro certo.

### Pendências para o CP1

- [ ] No boletim, nome do fornecedor sempre da `dimFornecedor`, pelo `cod_fornecedor`, nunca do texto digitado na carga.
- [ ] Na limpeza da base completa (Fase 7), padronizar com tabela de-para no Power Query, não com SUBSTITUIR encadeado.
