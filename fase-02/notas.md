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
