# Fase 1: O que é análise de dados

## Tópico 1: O que é análise de dados

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


Nas três decisões, o dado existia, mas chegava atrasado (D-1 ou laboratório externo) e espalhado em fontes diferentes (boletim, SAP e laboratório). Por isso, cada ajuste demorava a ser confirmado. Aprendi que achar o ponto fora da curva é só o começo: o valor está em entender o porquê, propor a ação e ter o dado rápido para ser validado.