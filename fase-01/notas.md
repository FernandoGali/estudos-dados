# Fase 1: O que é análise de dados

## Tópico 1: O que é análise de dados

### Teoria nas minhas palavras

> Escreva aqui, sem copiar, o que você entendeu: a diferença entre dado, informação, conhecimento e decisão (pirâmide DIKW), e o que um analista entrega. Use um exemplo da esmagadora.

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

> Uma ou duas frases: o que essas três decisões têm em comum do ponto de vista de dados?
