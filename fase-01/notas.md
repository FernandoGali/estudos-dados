# Fase 1: O que é análise de dados

## Tópico 1: O que é análise de dados

### Teoria nas minhas palavras

Dado sem análise não traz benefício nenhum. Pelo contrário: é custo para a empresa, com banco de dados e/ou com pessoas que o extraem todos os dias. Ele só gera valor quando sobe os degraus até virar decisão:

- Dado: o registro bruto. Ex.: turno III, 20/09, residual do farelo 0,71%.
- Informação: o dado comparado. Ex.: o residual médio do ano está em 0,82%, contra uma meta de 0,60%.
- Conhecimento: o porquê. Ex.: o turno III fica sempre acima dos outros.
- Decisão: a ação. Ex.: revisar o procedimento do extrator no turno III.

Na indústria de soja, não adianta olhar os indicadores todo turno sem agir, porque eles não vão se enquadrar sozinhos. O analista entrega relatórios do que aconteceu, análises do porquê e recomendações com impacto em número, para que a decisão seja mais assertiva.


### Prática: 3 decisões reais

#### Decisão 1: Ajuste de umidade do grão de entrada

- **Dados usados:** análise de qualidade do grão e relatório de consumo de solvente
- **Origem:** boletim de produção + relatório do SAP QM
- **Problema no dado:** o boletim de produção e o relatório do SAP QM fecham em D-1. Quando o consumo de solvente aparecia alto, a fábrica já tinha rodado um turno ou dia inteiro com o grão seco demais, e a correção da umidade só tinha efeito confirmado no boletim do dia seguinte.
- **Resultado:** notou-se que a umidade do grão estava muito baixa e, com isso, havia uma grande retenção de solvente no farelo, causando sobrecarga de solvente para o DT. Após a regularização, os níveis voltaram ao normal e o consumo de solvente baixou novamente.

#### Decisão 2: Alteração do ácido da degomagem enzimática

- **Dados usados:** paradas da centrífuga e análises laboratoriais
- **Origem:** boletim de paradas do SAP PM e análise laboratorial com ácido cítrico
- **Problema no dado:** a análise do grão é específica, não se faz em qualquer laboratório, e precisava ser enviada para um laboratório externo.
- **Resultado:** a troca do ácido cítrico pelo ácido lático não é 100% benéfica. Por um lado, houve redução drástica de paradas de fábrica relacionadas à centrífuga; por outro, o consumo de ácido aumentou. Ainda assim, a mudança é mais vantajosa do que prejudicial.

#### Decisão 3: Aumento dos pratos de dessolventização do DT

- **Dados usados:** alto consumo de solvente e residual de solvente no farelo final
- **Origem:** boletim de produção + balanço de massa
- **Problema no dado:** o consumo de solvente e o residual no farelo também saem em D-1. Cada ajuste nos pratos do DT só podia ser avaliado no dia seguinte, o que tornava o ciclo de teste lento.
- **Resultado:** o residual e o consumo de solvente ficaram dentro das metas, com o consumo caindo de cerca de 1,8 L/t para cerca de 1 L/t.


### O que aprendi

Nas três decisões, o dado existia, mas chegava atrasado (D-1 ou laboratório externo) e espalhado em fontes diferentes (boletim, SAP e laboratório). Por isso, cada ajuste demorava a ser confirmado. Aprendi que achar o ponto fora da curva é só o começo: o valor está em entender o porquê, propor a ação e ter o dado rápido para ser validado.

## Tópico 2: Análise descritiva

### Parte A: perguntas descritivas do boletim

1. Quantas toneladas de soja foram processadas no mês, e quanto isso representa da meta mensal?
2. Qual foi o índice acumulado de consumo de hexano (L/t) do mês? Está dentro da meta?
3. Qual foi a proteína média do farelo no mês, e qual foi a fibra média (indicador de quanta casca ficou no farelo)?
4. Qual foi o residual médio no LEX no mês? Comparado com a meta de 0,60%, quanto de óleo isso representou a mais ou a menos?
5. Quantas horas a fábrica parou no mês por parada não planejada, e como isso se compara com o mês anterior?

### Parte B: média simples x média ponderada (novembro de 2022)

| Índice | Média simples | Média ponderada |
|---|---|---|
| Vapor (kg/t) | 270,56 | 268,61 |
| Hexano (L/t) | 1,393 | 1,369 |

A média simples infla o vapor em cerca de 0,73% e o hexano em cerca de 1,75%.

**O que observei**

Nos 3 dias de menor produção (entre 573 e 990 t), o vapor ficou entre 271 e 294 kg/t e o hexano entre 1,56 e 1,62 L/t. Nos 3 dias de maior produção (entre 1.478 e 1.570 t), o vapor ficou entre 248 e 267 kg/t e o hexano entre 1,18 e 1,25 L/t, com uma exceção: no dia 24/11 o hexano chegou a 1,66 L/t mesmo com a fábrica cheia, o que aponta para uma causa de processo e não de carga. Outra exceção: no dia 26/11, com 1.431 t, o vapor chegou a 312 kg/t, o maior do mês.

Ou seja, nos dias com produção mais baixa, temos um consumo maior por tonelada, e isso pode ser por vários fatores: insumos necessários para rodar a planta, qualidade ruim de matéria-prima ou problemas de processo. Como a fábrica opera cheia na maior parte dos dias, a média simples infla pouco o índice do mês (0,73% no vapor e 1,75% no hexano). Ela só distorce de verdade em meses com muitas partidas e paradas.

O ideal de uma planta é o equilíbrio entre produção e consumo: achar o sweet spot em que se tem um maior rendimento a um baixo custo, sem ocasionar quebras ou paradas.

**Por que acontece**

Em dias com baixa produção, o índice de insumos fica elevado, pois existe um consumo mínimo para manter a planta rodando, e como a produção é baixa, isso distorce o índice.

**Qual método está certo**

A média simples dá o mesmo peso para dias com carga baixa e dias com carga total. A média ponderada é o índice real do período e é a que vai para o KPI e para o custo.

Os dias ruins não somem: eles aparecem na coluna do índice diário, que serve de alerta para investigação. E quando a média simples fica bem acima da ponderada, isso já sinaliza que os dias de baixa produção estão puxando o consumo para cima.
