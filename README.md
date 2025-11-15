# 1. Objetivo do Projeto:
O principal objetivo deste projeto é aplicar e comparar três modelos de classificação supervisionada – XGBoost, Support Vector Machine (SVM) com kernel RBF e Random Forest – no popular dataset Titanic. A meta é classificar se um passageiro sobreviveu (Survived = 1) ou não (Survived = 0) com base em diversas variáveis explicativas. A comparação focou em:

Desempenho: Avaliado principalmente pela métrica ROC-AUC, e secundariamente por Acurácia e F1-score.
Custo Computacional: Medido pelo tempo de treinamento de cada modelo.
Interpretabilidade: Através de gráficos como curvas ROC, Precision-Recall e matrizes de confusão.

# 2. Pré-processamento dos Dados:
Para preparar o dataset, foram realizadas as seguintes etapas:

Tratamento de Valores Ausentes: Preenchimento da coluna age com a mediana e da coluna embarked com o valor mais frequente (moda).
Escalonamento de Variáveis Numéricas: As colunas age e fare foram padronizadas usando StandardScaler, essencial para modelos baseados em distância como o SVM.
Codificação de Variáveis Categóricas: As colunas sex, embarked e pclass foram convertidas em formato numérico usando OneHotEncoder, criando novas colunas binárias para cada categoria.

# 3. Modelos Escolhidos e Otimização:
Os três modelos foram selecionados por suas diferentes abordagens e popularidade em tarefas de classificação:

Support Vector Machine (SVC): Um modelo linear e não-linear poderoso, utilizado aqui com o kernel RBF.
Random Forest: Um ensemble de árvores de decisão que geralmente oferece boa performance e robustez.
XGBoost: Um algoritmo de boosting gradient descent, conhecido por sua alta performance e eficiência.
Todos os modelos passaram por otimização de hiperparâmetros usando RandomizedSearchCV com StratifiedKFold (5 splits). Isso garante que a busca pelos melhores parâmetros seja eficiente e que a avaliação seja robusta e representativa da distribuição das classes, mesmo em um dataset potencialmente desbalanceado.

# 4. Resultados e Análise:
Os modelos foram avaliados no conjunto de teste, e os principais resultados foram (ordenados por test_roc_auc):
      	       
| Modelo	  | Best CV ROC-AUC | Train ROC-AUC  | Test ROC-AUC |  Test Accuracy  | Test F1 |  Fit Time (s)  |
| ------------- | ------------- | ------------- | ------------- | -------------  | ------------- | -------------  | 
| RandomForest  | 0.8697  |  0.9397	  | 0.8653  | 0.8172  | 0.7322   | 87.96   |
| XGBoost   | 0.8685  | 0.9346  | 0.8563	 | 0.8134  | 0.7222   | 11.14   |                                        
| SVC   | 0.8567  | 0.8997 | 0.8409  | 0.8060  | 0.7292   | 13.00   |


# Conclusões a partir dos resultados:
Melhor Desempenho Preditivo: O RandomForest obteve o maior ROC-AUC no conjunto de teste (0.8653), indicando a melhor capacidade de distinguir entre as classes de sobrevivência. O XGBoost ficou muito próximo, com 0.8563.
Eficiência Computacional: O XGBoost se destacou pelo seu tempo de treinamento significativamente menor (apenas 11.14 segundos), tornando-o uma escolha muito eficiente quando o tempo é um fator crítico.
Overfitting: Todos os modelos apresentaram um train_roc_auc maior que o test_roc_auc, sugerindo um leve overfitting. O Random Forest mostrou a maior diferença, mas em um nível aceitável para generalização. O SVC teve a menor diferença, mas com um desempenho de teste um pouco inferior.

# 5. Representação Visual:
As curvas ROC, Precision-Recall e as matrizes de confusão foram geradas para cada modelo, permitindo uma análise visual mais aprofundada do desempenho, especialmente para entender o balanço entre True Positives, False Positives, True Negatives e False Negatives. Por exemplo, as curvas ROC visualizam a capacidade de cada modelo de separar as classes em diferentes thresholds de probabilidade, e as matrizes de confusão mostram os erros e acertos em termos absolutos.

Discussão Geral (conforme seu notebook):
Melhor Modelo: O projeto conclui que o XGBoost foi o modelo com melhor equilíbrio, apresentando um desempenho muito próximo ao do Random Forest (o de maior AUC de teste), mas com um tempo de treinamento consideravelmente menor, o que o torna mais prático.
Overfitting: Houve sinais leves de overfitting em todos os modelos, com o Random Forest sendo o que menos sofreu em termos de generalização. O SVC apresentou um overfitting que foi um pouco inesperado comparado aos outros.
Dificuldades no Pré-processamento: Os principais desafios incluíram garantir que o pipeline de pré-processamento não vazasse informações, lidar com o possível desbalanceamento do dataset, e a necessidade do escalonamento para o SVC. A divisão de 30% para teste também foi identificada como um fator que poderia aumentar a chance de overfitting, exigindo atenção extra nos ajustes.
Em suma, o projeto demonstrou um pipeline completo de classificação supervisionada, desde o pré-processamento até a modelagem e avaliação, destacando a importância de comparar diferentes algoritmos e suas características.
