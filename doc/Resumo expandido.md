# Resumo Expandido
# Título: Comparativo de Modelos de Classificação Supervisionada para Previsão de Sobrevivência no Desastre do Titanic

# Autores e vínculo institucional: [Luis & Seliel] - [UMC - Universiade de Mogi das Cruzes]

# 1. Introdução
O naufrágio do Titanic em 1912 é um evento histórico frequentemente utilizado para estudos de análise de dados, servindo como um excelente dataset para a aplicação e comparação de modelos de classificação supervisionada. Este trabalho visa prever a sobrevivência de passageiros (survived) com base em características demográficas e de viagem, empregando três algoritmos distintos: XGBoost, Support Vector Machine (SVM) com kernel RBF e Random Forest. O objetivo é não apenas alcançar um alto poder preditivo, mas também comparar o desempenho, custo computacional e interpretabilidade desses modelos, fornecendo insights sobre qual deles oferece o melhor balanço para a tarefa proposta.

# 2. Fundamentação Teórica do Modelo Escolhido (XGBoost)
Dentre os modelos avaliados, o XGBoost (eXtreme Gradient Boosting) emergiu como a melhor opção devido ao seu equilíbrio entre alta performance e eficiência. O XGBoost é uma implementação otimizada de bibliotecas de boosting gradient descent, que se baseia na construção sequencial de modelos de decisão (árvores). Cada nova árvore é adicionada para corrigir os erros das árvores anteriores, utilizando um gradiente descendente para minimizar uma função de perda. Ele incorpora técnicas de regularização (L1 e L2) para evitar overfitting, manipula valores ausentes intrinsecamente e utiliza otimizações de hardware e paralelização que o tornam excepcionalmente rápido e escalável. Sua capacidade de construir modelos complexos e robustos, ao mesmo tempo em que controla o overfitting, o torna uma escolha poderosa para problemas de classificação como este.

# 3. Metodologia Computacional
Pré-processamento
O dataset Titanic foi submetido às seguintes etapas de pré-processamento:

Tratamento de Valores Ausentes: Valores faltantes na coluna age foram preenchidos com a mediana, e na coluna embarked com a moda.
Escalonamento de Variáveis Numéricas: age e fare foram padronizadas usando StandardScaler, crucial para o bom funcionamento do SVM.
Codificação de Variáveis Categóricas: As features sex, embarked e pclass foram convertidas em representações numéricas binárias utilizando OneHotEncoder.
Hiperparâmetros Utilizados
Para cada modelo, uma distribuição de hiperparâmetros foi definida e explorada usando RandomizedSearchCV. Exemplos de parâmetros chave incluem:

SVC: C (força da regularização), gamma (influência do kernel).
RandomForestClassifier: n_estimators (número de árvores), max_depth (profundidade máxima), min_samples_split, min_samples_leaf, max_features.
XGBoostClassifier: n_estimators, max_depth, learning_rate, subsample, colsample_bytree, reg_alpha, reg_lambda.
Engenharia de Atributos
As variáveis explicativas (pclass, sex, age, fare, sibsp, parch, embarked) foram selecionadas a partir do dataset bruto. A engenharia de atributos focou na transformação dessas colunas para um formato adequado aos modelos de Machine Learning, como a criação de variáveis dummy através do OneHotEncoder para sex, embarked e pclass, e a padronização para age e fare.

Validação Cruzada
A divisão dos dados foi realizada utilizando train_test_split (70% treino, 30% teste) com stratify=y para garantir a mesma proporção da variável alvo em ambos os conjuntos. A validação cruzada para a otimização de hiperparâmetros foi feita com StratifiedKFold (n_splits=5), assegurando que cada fold mantivesse a proporção original das classes, o que é fundamental para datasets potencialmente desbalanceados.

Comparação com os Modelos de Referência (XGBoost, SVM, Random Forest)
Os resultados de desempenho no conjunto de teste, ordenados pelo Test ROC-AUC, foram:

Modelo	Best CV ROC-AUC	Train ROC-AUC	Test ROC-AUC	Test Accuracy	Test F1	Fit Time (s)
RandomForest	0.8697	0.9397	0.8653	0.8172	0.7322	87.96
XGBoost	0.8685	0.9346	0.8563	0.8134	0.7222	11.14
SVC	0.8567	0.8997	0.8409	0.8060	0.7292	13.00

# 4. Análise da Importância dos Atributos
Para os modelos baseados em árvore (Random Forest e XGBoost), a análise da importância das features (através de feature_importances_) revelou que as variáveis relacionadas ao sexo (sex_male, sex_female), à classe do passageiro (pclass_1, pclass_3) e à tarifa (fare) foram consistentemente as mais influentes na previsão de sobrevivência. A idade (age) também demonstrou um papel significativo. Esta análise sugere que fatores socioeconômicos e de gênero tiveram um peso considerável no desfecho da sobrevivência no Titanic.


# 5. Análise da Importância dos Hiperparâmetros
A busca por hiperparâmetros via RandomizedSearchCV foi crucial para otimizar o desempenho dos modelos. Para o XGBoost, parâmetros como n_estimators (número de árvores) e max_depth (profundidade das árvores) são fundamentais, pois controlam a complexidade do modelo e seu potencial de overfitting. A learning_rate define a contribuição de cada árvore e é vital para o processo de boosting. Para o Random Forest, n_estimators também é chave, enquanto max_depth e max_features controlam a diversidade e a força das árvores individuais. No SVC, C (parâmetro de regularização) e gamma (influência do kernel) determinam a margem de separação e a flexibilidade do modelo, respectivamente. A escolha adequada desses parâmetros permite um balanço entre bias e variância, maximizando a capacidade de generalização do modelo.

# 6. Discussão e Justificativa do Melhor Modelo
Com base na comparação, o XGBoost é justificado como o melhor modelo para este problema. Embora o Random Forest tenha apresentado um Test ROC-AUC ligeiramente superior, o XGBoost alcançou um desempenho quase idêntico com um tempo de treinamento aproximadamente 7 vezes menor (11.14s vs 87.96s). Esta eficiência computacional é um diferencial crítico, especialmente em cenários com volumes de dados maiores ou requisitos de deploy rápidos. Além disso, o XGBoost demonstra robustez e um bom controle de overfitting através de suas técnicas de regularização intrínsecas, o que o torna uma escolha pragmática e de alta performance.

# 7. Conclusões
O projeto demonstrou um processo completo de modelagem de classificação supervisionada no dataset Titanic. Através do pré-processamento cuidadoso, otimização de hiperparâmetros e avaliação rigorosa, foi possível identificar o XGBoost como o modelo mais eficiente e com melhor custo-benefício. A análise de importância de atributos corroborou que sexo, classe e tarifa foram determinantes para a previsão de sobrevivência. Os desafios incluíram o manejo de valores ausentes e o ajuste fino para mitigar o overfitting. Os resultados fornecem uma base sólida para futuras investigações, como a exploração de técnicas de ensemble mais avançadas ou o desenvolvimento de atributos mais complexos.

 ***Dataset Titanic (disponível via seaborn ou Kaggle).***
