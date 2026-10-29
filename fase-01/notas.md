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

## Tópico 5: Análise prescritiva

### Parte A: quanto vale reduzir o óleo no farelo final

**Contexto:** a fábrica produz 30.000 t de farelo por mês. A goma separada na degomagem volta para o farelo e leva óleo junto. Na degomagem enzimática, as enzimas convertem parte da goma em óleo, então a goma carrega menos óleo e o incremento de óleo no farelo final diminui em relação à degomagem aquosa. A pergunta é: quanto vale, em toneladas e em reais, essa redução?

**Simplificação adotada:** na prática, o óleo do farelo final depende do LEX (óleo no farelo branco, na saída do extrator), do Fator DT (que concentra o teor de óleo ao evaporar o hexano) e do que é adicionado ao farelo depois (casca, goma). Para este estudo, considerei a variação direto no óleo do farelo final.

**Premissas**

| Premissa | Valor | Origem |
|---|---|---|
| Produção de farelo | 30.000 t/mês | Premissa do exercício |
| Óleo no farelo final, degomagem aquosa | 2,0 a 2,5% (ponto médio 2,25%) | Experiência de fábrica |
| Óleo no farelo final, degomagem enzimática | 1,0 a 1,5% (ponto médio 1,25%) | Experiência de fábrica |
| Preço do óleo degomado | R$ 7.941,38 /t | Cotação de Chicago (CBOT) convertida para R$/t em 27/10/2026, com o câmbio do dia |
| Preço do farelo | R$ 2.082,38 /t | Cotação de Chicago (CBOT) convertida para R$/t em 27/10/2026, com o câmbio do dia (farelo cotado em short ton) |

**Conta (pontos médios)**

- Óleo recuperado por mês = 30.000 t × (2,25% − 1,25%) = **300 t/mês**
- Valor bruto por mês = 300 t × R$ 7.941,38 = R$ 2.382.414,00
- Ganho real por mês = 300 t × (R$ 7.941,38 − R$ 2.082,38) = **R$ 1.757.700,00**

**Óleo x farelo:** o óleo recuperado deixa de ser vendido dentro do farelo, como farelo. Por isso o ganho real é a diferença entre os dois preços, e não o preço cheio do óleo. Usar o valor bruto superestimaria o ganho em cerca de R$ 625 mil por mês.

**Cenários**

| Cenário | Redução do teor | Óleo recuperado | Ganho por mês |
|---|---|---|---|
| Pessimista (2,0% → 1,5%) | 0,5 p.p. | 150 t | R$ 878.850,00 |
| Médio (2,25% → 1,25%) | 1,0 p.p. | 300 t | R$ 1.757.700,00 |
| Otimista (2,5% → 1,0%) | 1,5 p.p. | 450 t | R$ 2.636.550,00 |

**Por ano:** com 12 meses de operação, R$ 21,09 milhões. Como a fábrica opera em campanha e para na entressafra e por falta de matéria-prima, uma estimativa mais honesta usa cerca de 10 meses: **R$ 17,58 milhões por ano** no cenário médio.

**Limites da conta:** é ganho bruto. Para ser o ganho líquido, falta descontar o custo da enzima e dos reagentes da degomagem enzimática, e o investimento, se houver. Também depende de o farelo continuar dentro da especificação comercial.

### Parte B: o que fazer com a parada de novembro de 2022

Cenário: em 24/10/2022, a previsão do Tópico 4 diz que a soja acaba por volta de 02/11 e a fábrica deve ficar parada cerca de 15 dias. Planilha: `topico5_parteB_parada_nov2022.xlsx`.

| Opção | Ganho | Custo ou risco |
|---|---|---|
| 1. Aceitar a parada e usá-la para a preventiva e as manutenções pendentes | Antecipa manutenções obrigatórias (extração) com a fábrica parada | 15 dias sem produzir: cerca de 4.300 t de óleo e 15.600 t de farelo a menos |
| 2. Comprar soja no mercado à vista para não parar | Mantém a produção. A parada seria por falta de matéria-prima e há armazém disponível | Soja à vista mais cara que a contratada; logística de cerca de 27 caminhões a mais por dia; adiar manutenção pode agravar problemas e gerar uma parada maior depois |
| 3. Reduzir a carga para o estoque durar mais | Estende os dias de operação | Só adia a parada (cerca de 16 dias em vez de 9 a 1.000 t/dia). Abaixo da carga nominal, o consumo de vapor, energia e hexano por tonelada sobe |

**Soja necessária para não parar em 15 dias (opção 2):** (1.438,51 − 440,66) t/dia × 15 dias = **14.967,75 t**, cerca de 405 caminhões de 37 t, ou cerca de 27 caminhões por dia além do normal.

**Regra de decisão:** a opção 2 é melhor para o faturamento se as pendências de manutenção forem só preventivas. Se houver manutenção crítica (risco de quebra, risco de segurança ou perda de rendimento em andamento), a opção 1 é melhor. A opção 3 não resolve: só adia a parada e encarece a produção.

**O que decide:** a lista de ordens de manutenção pendentes com a criticidade (SAP PM) e a cotação da soja à vista comparada com a margem de esmagamento.

### Parte C: recomendação

E-mail ao gerente industrial:

> **Assunto:** Estoque de soja acaba em 02/11, decisão até 27/10
>
> No ritmo atual (cerca de 1.440 t/dia esmagadas e 440 t/dia recebidas), a soja acaba por volta de 02/11 e a fábrica para cerca de 15 dias.
> Temos três opções: (1) parar e antecipar a manutenção; (2) comprar cerca de 15.000 t de soja à vista e não parar; (3) reduzir a carga, o que só adia a parada (cerca de 16 dias em vez de 9).
> Parar significa deixar de produzir cerca de 4.300 t de óleo e 15.600 t de farelo. Não parar exige pagar o valor da soja à vista e receber cerca de 27 caminhões a mais por dia.
> Recomendo a opção 2, desde que não haja manutenção crítica pendente. Se houver, a opção 1.
> Para fechar a decisão, preciso até 27/10 da lista de ordens de manutenção pendentes com a criticidade (Manutenção) e da cotação da soja à vista (Originação). Se seguirmos a opção 2, acompanho diariamente os dias de estoque e os caminhões recebidos.

| Parte | Onde está no e-mail |
|---|---|
| Situação | Linha 1: quando a soja acaba e quanto tempo a fábrica para |
| Opções | Linha 2 |
| Impacto | Linha 3: o que se perde parando e o que custa não parar |
| Recomendação | Linha 4: regra de decisão condicional |
| Como acompanhar | Linha 5: quem entrega o quê, até quando, e o indicador diário |

## Tópico 6: O ciclo do dado

**Teoria:** toda análise passa por seis etapas em ciclo: pergunta, coleta, limpeza, exploração, visualização e comunicação. A decisão gera novas perguntas e o ciclo recomeça. O ciclo é tão bom quanto a etapa mais fraca, e a coleta quase sempre acontece longe do analista (turno, balança, laboratório).

### Parte A: paralelo com PDCA e DMAIC

| Ciclo do dado | PDCA (aplicado ao problema da fábrica) | DMAIC |
|---|---|---|
| Pergunta | Plan | Define |
| Coleta | Plan | Measure |
| Limpeza | Plan | Measure (validar o sistema de medição, como no MSA) |
| Exploração | Plan | Analyze |
| Visualização | Plan e Check | Analyze e Control |
| Comunicação | Plan, passando para o Do | Fim do Analyze, passando para o Improve |
| (sem equivalente: quem age é a operação) | Do | Improve |
| Medir de novo | Check e Act | Control |

**Outra lente:** aplicando o PDCA ao próprio trabalho de análise, fica: Plan = definir as perguntas; Do = coletar, limpar e explorar; Check = conferir os indicadores no painel; Act = comunicar insights e alertas. As duas leituras são válidas; numa entrevista, dizer qual lente estou usando.

**Qual etapa do DMAIC não tem equivalente:** o Improve. O analista não executa a melhoria, ele a recomenda. Se o gestor recebe o alerta de que um equipamento tende a quebrar e ninguém age, a análise não gerou valor e o problema volta a aparecer nos dados. Por isso a entrega precisa dizer o que fazer, quem faz e até quando, e trazer o indicador que vai mostrar, no Control, se a ação funcionou.

**Limpeza é o meu Measure:** antes de analisar, garanto que o dado mede o que diz medir.

### Parte B: o ciclo do boletim diário de uma esmagadora

| Etapa | Quem faz | Quando | Ferramenta | Onde dá erro ou atraso |
|---|---|---|---|---|
| 1. Medição e registro no setor | Operadores e líderes | Durante o turno | Boletins dos setores, medidores, balança | Medidor descalibrado; leitura anotada errada |
| 2. Coleta e lançamento (produção, manutenção, utilidades) | Encarregados, com o PCM na manutenção | Fim do turno | SAP | Digitação errada, campo em branco, atraso por o dado chegar tarde ou por demora no lançamento |
| 3. Lançamento da qualidade | Analistas do laboratório | Fim do turno | SAP | Amostra irregular, erro de análise, atraso no início; resultado lançado depois, com o dia já fechado |
| 4. Conferência | Coordenador de cada área | Após o lançamento | SAP | Conferência manual: depende de quem confere perceber o erro |
| 5. Distribuição | Analistas e líderes | Fim do turno | Visita ao setor, e-mail, boletim impresso | Três canais, sem uma versão única |
| 6. Decisão | Coordenadores e gerência | Quando necessário | Reunião ou conversa | Sem gatilho definido: a decisão depende de alguém notar |

**Fontes de cada variável:** produção (medidores, balança, medição de tanque de hexano), manutenção (controle de paradas por motivo), utilidades (medidor de vazão de vapor, substação), qualidade (laboratório). Três tipos de falha na origem: medição (instrumento descalibrado), registro (apontamento) e amostragem/análise (laboratório). Categoria "outras paradas" genérica demais: o dado existe, mas não serve para análise.

### Parte C: os 2 pontos onde o ciclo mais falha

**1. Resultado do laboratório que chega depois do fechamento.** O dia fecha sem a análise e é corrigido depois; quem consulta antes vê um dado incompleto.
- Solução rápida (indicador e processo): marcar o dado como provisório até o laboratório lançar e acompanhar a pontualidade (% de análises lançadas até o fim do turno).
- Solução estrutural (sistema e automação): NIR em linha para umidade, proteína e óleo, calibrado contra o laboratório. Reduz a quantidade de análises manuais e dá o dado em tempo real.

**2. Consumo de hexano medido à mão no tanque.** É o índice mais caro e o mais exposto a erro de leitura; a diferença de nível entre leituras mistura períodos.
- Solução rápida (do analista, com o dado que já existe): conciliação mensal com as compras. Hexano comprado − variação do estoque do tanque = hexano consumido de fato. Se a soma dos consumos lançados no mês for muito diferente desse número, a medição diária tem erro sistemático. Usa uma fonte independente (notas de compra) e não depende de o encarregado conferir nada. No dia a dia, o analista marca no próprio relatório os dias fora do padrão (carta de controle do índice), em vez de pedir conferência no lançamento.
- Por que não um alerta no lançamento: o encarregado confia na própria medição ("faço isso todo dia") e lança mesmo assim. Solução que exige mudança de rotina sem ganho para quem executa costuma falhar.
- Solução estrutural (instrumentação): medidor de vazão na reposição de hexano, com leitura automática no sistema. Mais acurácia e menos risco da medição manual.

**Lição:** propor uma solução para amanhã, com o dado que já existe, e uma para o ano que vem, com investimento.

## Tópico 7: A pergunta de negócio

**Teoria:** pergunta boa é específica (o quê, onde, quando), mensurável (dá para responder com um número a partir de dados que existem) e ligada a uma decisão. Teste: "se a resposta for X, faremos A; se for Y, faremos B". Se não dá para completar a frase, a pergunta não está pronta. O gestor quase sempre chega com a pergunta vaga; transformá-la é trabalho do analista ("para decidir o quê?", "comparado com o quê?", "em que período?").

**Armadilhas que cometi e corrigi:**
- A média esconde o que está fora da especificação: perguntar também o % fora do padrão
- A média de todos esconde o indivíduo: fornecedor ruim some no meio de 160
- Consumo total engana: comparar por tonelada (kWh/t), não em absoluto
- "Muito acima" não é mensurável: definir o limite
- Comparar só com o mês passado é fraco: usar a meta como gatilho e o ano anterior como contexto
- Citar colunas pelo nome, não pela letra da planilha

### 1. "Como está a qualidade do farelo?"

**Pergunta:** no último mês, qual a proteína média do farelo e qual o % de análises abaixo de 46%, por turno?

**Decisão:** se a média estiver acima de 46,3% e quase nenhuma análise abaixo, aumentamos a adição de casca (estamos entregando proteína de graça). Se mais de 5% das análises estiverem abaixo, reduzimos a casca e investigamos o turno com mais desvios.

**Dados:** `fato_lab_farelo` (`proteina_pct`, `turno`, `data`).

### 2. "Os fornecedores estão bons?"

**Pergunta:** entre os fornecedores que somam 80% do volume em 2026, quais têm o maior % de desconto de classificação (`desconto_t` ÷ `peso_liquido_t`)?

**Decisão:** se um fornecedor grande estiver muito acima da média, a originação negocia a qualidade na renovação do contrato. Se for um fornecedor pequeno, ele perde prioridade nas compras. Trocar um fornecedor grande sem olhar o volume pode faltar soja.

**Dados:** `fato_recebimento` (`id_fornecedor`, `peso_liquido_t`, `desconto_t`, `umidade_pct`, `impureza_pct`) + `dim_fornecedor`.

### 3. "Estamos gastando muita energia?"

**Pergunta:** qual foi o consumo de energia por tonelada esmagada (kWh/t) em outubro de 2026, quanto % acima ou abaixo da meta, e como se compara a outubro de 2025?

**Decisão:** se estiver mais de 5% acima da meta, recalculamos só com os dias de operação normal (20 h ou mais trabalhadas). Se ainda estiver acima, abrimos investigação na preparação e na extração, os maiores consumidores. Se o excesso sumir sem os dias de partida, o problema é o número de paradas, não a eficiência.

**Dados:** `fato_producao` (`energia_kwh`, `soja_esmagada_t`, `horas_trabalhadas`, `data`).
