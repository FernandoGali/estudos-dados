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

## Tópico 3: Análise diagnóstica

Caso investigado: hexano de 1,66 L/t no dia 24/11/2022, com a fábrica cheia. Planilha de apoio: `investigacao_hexano_24-11-2022.xlsx`.

### Parte A: árvore de hipóteses (4M + Medida)

| Categoria | Hipótese | Dado que confirmaria ou descartaria | Existe na base? |
|---|---|---|---|
| Matéria-prima | Entrada de grão muito seco no processo | Umidade da soja industrializada no dia 24/11 comparada com os dias anteriores | Sim |
| Mão de obra | A impureza muda a densidade da massa e gera variação de carga; sem treinamento, o operador não ajusta o equipamento | Impureza da soja industrializada; comparação entre turnos; equipe do turno | Em parte |
| Método | Alta velocidade na rosca de descarga do DT | Velocidade de rotação da rosca; residual de solvente no farelo | Não |
| Máquina | Ejetor da destilaria entupido ou com passagem de ar | Tendência do índice nos dias anteriores; medidor de vácuo; ordem de manutenção | Em parte |
| Medida | PT100 da cabeça do DT descalibrado, lendo acima do real; com o DT frio, o solvente não recuperado sai no farelo | Aferição do PT100; cronograma de calibração | Não |

### Parte B: teste com os dados

| Hipótese | Resultado | Evidência |
|---|---|---|
| Grão muito seco | Descartada | Umidade de 10,35% no dia 24/11, dentro da faixa de 10,07% a 10,90% dos dias 20 a 23/11 |
| Impureza e variação de carga | Descartada | Impureza de 1,03% no dia 24/11, uma das menores da semana (0,90% a 1,31%). Um fator que não mudou no dia 24 não explica um pico que só aparece nele |
| Ejetor entupido | Fortalecida | Índice subindo dia a dia de 1,18 L/t (20/11) para 1,66 L/t (24/11), depois de 15 dias de fábrica parada; queda para 1,37 e 1,31 L/t após a preventiva de 25/11 |
| Velocidade da rosca do DT | Não testável | A base não tem velocidade da rosca nem residual de solvente no farelo |
| PT100 descalibrado | Não testável | A base não tem temperatura do DT nem histórico de calibração |

Outras observações:
- O dia 19/11 também teve índice alto (1,59 L/t), mas foi dia de partida (14,5 h rodando), o que explica o valor.
- No dia 24/11, o residual de óleo no farelo foi de 0,75% no turno III, contra 0,62% no turno I e 0,61% no turno II.
- A quebra de corrente do dia 25/11 foi na RSC-01 (rosca transportadora de farelo), sem relação com a recuperação de solvente.

### Parte C: conclusão sobre o dia 24/11/2022

**Problema:** hexano de 1,66 L/t no dia 24/11, com a fábrica rodando 24 h (1.507 t), contra 1,18 a 1,25 L/t nos outros dias cheios da campanha.

**Descartado:** umidade do grão (10,35%, dentro da faixa de 10,07% a 10,90% dos dias anteriores) e impureza (1,03%, uma das menores da semana). Um fator que não mudou no dia 24 não explica um pico que só aparece nele.

**Mais provável (fortalecida, não confirmada):** entupimento progressivo do ejetor da destilaria. A fábrica ficou parada de 04 a 18/11, o que favorece o entupimento. Depois da partida, o índice subiu dia a dia de 1,18 L/t (20/11) para 1,66 L/t (24/11). Após a manutenção preventiva de 25/11, caiu para 1,37 e 1,31 L/t, o que sugere que o ejetor pode ter sido limpo nessa parada.

**Não testado:** PT100 da cabeça do DT e velocidade da rosca de descarga. A base não tem esses dados, então essas hipóteses continuam abertas.

**Para confirmar, eu pediria:**
- Ordem de manutenção da preventiva de 25/11 (SAP PM, Manutenção)
- Histórico do vácuo da destilaria de 19 a 27/11 (supervisório, Operação)

## Tópico 4: Análise preditiva

### Parte A: o que vale prever numa esmagadora

| O que prever | Decisão que apoia | Dados necessários | O SAP faz? |
|---|---|---|---|
| Quanto óleo vamos vender no mês? Quanto precisaremos produzir? | Programação de produção e manutenção | Histórico de vendas, contratos já fechados e sazonalidade | Registra as vendas, mas a previsão depende de extrair e analisar os dados |
| Por quantos dias o estoque de enzima dura com a produção cheia? | Reabastecimento de enzimas | Estoque atual, dosagem por tonelada de óleo, produção programada e lead time do fornecedor | A lista técnica tem o consumo teórico, mas os apontamentos são manuais; serve para o aprovisionamento e para checar se há estoque para produzir, não para prever o consumo real |
| A soja comprada, somada ao estoque, vai ser suficiente para rodar o mês todo? Se não, quando vamos parar? | Programação de produção e manutenção | Recebimento diário, estoque, esmagamento programado e compras já contratadas | Recebe as informações, mas não projeta a data em que a soja acaba; isso só aparece ao criar a ordem de produção |

### Parte B: previsão da parada de novembro de 2022, feita em 24/10/2022

Usando só os dados disponíveis até 24/10/2022 (premissa: o silo precisa de no mínimo 300 t para operar):

| Dado | Valor |
|---|---|
| Estoque de soja no fechamento de 24/10 | 9.240,72 t |
| Esmagamento médio de 18 a 24/10 | 1.438,51 t/dia |
| Recebimento líquido médio de 18 a 24/10 (peso líquido 3.118,22 t − desconto 33,59 t, dividido por 7) | 440,66 t/dia |

| Previsão | Conta | Dias | Data em que a soja acaba |
|---|---|---|---|
| Ingênua (sem recebimento) | (9.240,72 − 300) ÷ 1.438,51 | 6,21 | 30/10 |
| Com recebimento | (9.240,72 − 300) ÷ (1.438,51 − 440,66) | 8,96 | Fim de 01/11 |

**O que aconteceu de fato:** a última operação foi no dia 03/11, com carga parcial (990 t). A fábrica parou no dia 04/11.

| Previsão | Erro |
|---|---|
| Ingênua | Cerca de 4 dias cedo, porque ignorou a soja que continuou chegando |
| Com recebimento | Cerca de 1 a 1,5 dia cedo |

**Por que a previsão com recebimento errou para mais cedo:** ela usou médias, e os dias seguintes foram diferentes da média. No dia 26/10, uma parada fez a fábrica esmagar só 882 t, sobrando cerca de 550 t de soja no silo. No dia 03/11, chegaram mais de 1.000 t, o dobro da média. As duas coisas empurraram a parada para mais tarde. Por isso, na prática, a previsão é refeita todo dia com o dado novo.

### Parte C: limites e uso

**O que poderia fazer a previsão errar**
- O recebimento dos próximos dias ser diferente da média (chuva, fim de semana, atraso de contrato ou pico de entrega)
- Uma quebra inesperada de equipamento, que reduz o esmagamento (como no dia 26/10)
- O armazém de farelo encher antes: a fábrica para por falta de espaço mesmo com soja no silo. A previsão olha uma restrição só, e a fábrica tem várias

**O que eu faria no PCP no dia 24/10, sabendo que a soja acaba por volta de 02/11**
- Acionar a manutenção para usar a parada nas manutenções pendentes
- Organizar a compra de insumos, para que os estoques estejam prontos na retomada
- Se houver entregas de produto acabado pendentes, avisar o comercial sobre a necessidade de compra de soja

A previsão transforma uma parada imprevista numa janela planejada. Com 9 dias de antecedência, dá para abrir as ordens no SAP PM, separar peças e escalar equipes.

**Bônus: quando a fábrica volta.** Com a regra de retomar só com soja para 5 dias de operação (cerca de 7.300 t, mais as 300 t mínimas) e um recebimento de cerca de 440 t/dia, a conta dá por volta de 15 a 17 dias parada. Na base, a fábrica ficou parada de 04 a 18/11 (15 dias). Isso diz à manutenção quanto tempo ela tem, e não só quando a parada começa.
