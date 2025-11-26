# Classificação de Risco Cardiovascular com Regressão Logística

1. Visão Geral do Projeto
Este projeto utiliza o modelo de Regressão Logística para classificar clientes com base em dados de saúde, prevendo a probabilidade de desenvolver doença cardiovascular (cardio: 1 - Sim, 0 - Não).

O foco principal é o pré-processamento (Feature Engineering), essencial para adequar a base de saúde ao modelo, e a avaliação do desempenho do classificador, utilizando métricas como AUC-ROC para validar a capacidade do modelo de distinguir entre clientes saudáveis e clientes de risco.

2. Pipeline de Feature Engineering e Preparação

A base CARDIO_BASE.csv exige passos de tratamento específicos para transformar variáveis de saúde em features utilizáveis.

Conversão de Idade: A idade foi fornecida em dias. O código a converteu para anos (age_years = age / 365.25), o que é fundamental para a interpretação humana e para o modelo.

Conversão de Altura/Peso para IMC:
Cálculo: O Índice de Massa Corporal (IMC) é calculado usando a fórmula: $IMC = \frac{peso\_em\_kg}{altura\_em\_metros^2}$.
Justificativa: O IMC é um preditor de saúde muito mais informativo do que a altura e o peso isoladamente, transformando duas features correlacionadas em uma única variável de alta relevância clínica.

Tratamento de Outliers de Pressão Arterial: Foi aplicado um filtro de remoção de outliers para garantir a qualidade dos dados:

Remoção de registros onde a pressão diastólica (ap_lo) era maior que a pressão sistólica (ap_hi).

Remoção de registros com pressões sistólicas ou diastólicas extremamente baixas (ex: < 50).

Justificativa: Dados de saúde com valores fisicamente impossíveis ou clinicamente irrelevantes poluem o modelo e introduzem viés.

Divisão de Bases e Scaling: A base é dividida em Treino/Teste (70/30) e o StandardScaler é aplicado às features numéricas.

Justificativa: O scaling é obrigatório para a Regressão Logística para que os coeficientes não sejam dominados por features com grandes magnitudes.

3. Modelagem e Avaliação (Regressão Logística)
Algoritmo Escolhido: LogisticRegression.

Justificativa: É um modelo robusto, rápido, e que fornece uma probabilidade de risco (o que é ideal para a área da saúde e finanças) e é altamente interpretável via coeficientes.

Treinamento: O modelo (model_lr) é treinado nos dados escalados (X_train_scaled, y_train).

Previsão de Probabilidades: São geradas as probabilidades (y_proba) e as classes (y_pred) na base de teste.

4. Análise de Desempenho com Curva ROC e AUC

A avaliação do modelo de classificação é focada na métrica AUC-ROC (Area Under the Receiver Operating Characteristic Curve).

O que é a Curva ROC?

É um gráfico que mostra o desempenho do classificador em todos os limiares de decisão possíveis.

Compara a Taxa de Verdadeiros Positivos (TPR/Recall/Sensibilidade) no eixo Y com a Taxa de Falsos Positivos (FPR) no eixo X.

O que é o AUC?
É a área total sob a curva ROC.
O valor do AUC varia de 0 a 1.
AUC = 0.5: O modelo é tão bom quanto um chute aleatório.
AUC = 1.0: O modelo é perfeito.
Resultado Típico: O código deve retornar um AUC acima de 0.5 (ex: $\approx 0.72$), indicando que o modelo tem um bom poder de discriminação.
Interpretação: Um AUC de 0.72 significa que há 72% de probabilidade de o modelo classificar corretamente um par de instâncias (uma positiva e uma negativa).

5. Conclusão Final

O projeto demonstra que a Regressão Logística, combinada com um Feature Engineering de qualidade, é eficaz na classificação do risco cardiovascular.

O valor de AUC-ROC valida a capacidade preditiva do modelo, mostrando que ele consegue diferenciar clientes saudáveis daqueles em risco de forma significativa.

A saída do modelo (probabilidades) permite à área da saúde definir intervenções preventivas com base no nível de risco de cada paciente.

Pontos em Comum entre Regressão Logística e Regressão Linear (Perguntas Teóricas)
Apesar de a Logística ser um modelo de Classificação e a Linear ser de Regressão, elas compartilham pontos fundamentais:

Combinação Linear das Variáveis: Ambos os modelos calculam, inicialmente, uma combinação linear ponderada das features de entrada:
$$Z = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + ... + \beta_n x_n$$

$\beta_i$ são os Coeficientes (pesos), e $\beta_0$ é o Intercept.
O modelo busca os coeficientes que melhor se ajustam aos dados de treinamento.
Interpretação de Coeficientes: Nos dois modelos, os coeficientes $\beta_i$ representam a importância e a direção da relação de cada feature ($x_i$) com a saída final.
A diferença é o que acontece com $Z$: a Regressão Linear retorna $Z$ diretamente, e a Regressão Logística aplica a função Sigmoide a $Z$ para transformá-lo em uma probabilidade entre 0 e 1.
