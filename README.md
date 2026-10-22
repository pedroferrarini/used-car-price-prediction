# Previsão de Preços de Carros Usados

## Sobre o projeto

Este projeto foi desenvolvido para a Rusty Bargain, um serviço de venda de carros usados que deseja implementar um aplicativo capaz de estimar rapidamente o valor de mercado de veículos.

O objetivo foi desenvolver e avaliar modelos de Machine Learning para prever preços com base em características técnicas e informações dos veículos, considerando três critérios principais:

- Qualidade das previsões.
- Tempo de treinamento.
- Tempo necessário para realizar previsões.

## Objetivos

- Preparar e limpar os dados históricos de veículos.
- Realizar análise exploratória e identificar valores inconsistentes.
- Treinar diferentes modelos de regressão.
- Ajustar hiperparâmetros para melhorar o desempenho.
- Comparar os modelos considerando precisão e velocidade.
- Avaliar o modelo selecionado em um conjunto de teste independente.

## Tecnologias utilizadas

- Python
- Pandas
- Scikit-learn
- LightGBM
- CatBoost
- Matplotlib
- Jupyter Notebook

## Conjunto de dados

O projeto utiliza o arquivo `car_data.csv`, com informações históricas de anúncios de carros usados.

Entre as principais variáveis estão:

| Variável | Descrição |
|---|---|
| `Price` | Preço do veículo, variável-alvo |
| `VehicleType` | Tipo de veículo |
| `RegistrationYear` | Ano de registro |
| `Gearbox` | Tipo de câmbio |
| `Power` | Potência do motor |
| `Model` | Modelo do veículo |
| `Mileage` | Quilometragem |
| `FuelType` | Tipo de combustível |
| `Brand` | Marca |
| `NotRepaired` | Indicação de reparos não realizados |
| `RegistrationMonth` | Mês de registro |
| `PostalCode` | Código postal |

## Etapas do projeto

### 1. Preparação e limpeza dos dados

A base original continha 354.369 registros. Durante a preparação, foram realizadas as seguintes etapas:

- Identificação e tratamento de valores ausentes nas variáveis categóricas, utilizando a categoria `unknown`.
- Remoção de 262 registros duplicados.
- Exclusão de anúncios com preço igual a zero.
- Filtragem dos anos de registro para o intervalo de 1900 a 2016.
- Remoção de veículos com potência igual a zero ou superior a 600 hp.
- Exclusão de colunas relacionadas às datas dos anúncios e de uma coluna sem variação relevante para o treinamento.

Para os modelos que exigem variáveis numéricas, as categorias foram transformadas por meio de One-Hot Encoding. O CatBoost foi treinado com as variáveis categóricas originais.

### 2. Treinamento e validação

Os dados foram divididos em três conjuntos:

- **Treinamento:** 60%.
- **Validação:** 20%.
- **Teste:** 20%.

A divisão foi realizada com `train_test_split` e `random_state=12345`.

O conjunto de validação foi utilizado para comparar os modelos e ajustar seus hiperparâmetros. O conjunto de teste foi reservado para a avaliação final do modelo selecionado.

### 3. Modelos avaliados

Foram avaliados cinco algoritmos de regressão:

- Regressão Linear — modelo de referência (*baseline*).
- Árvore de Decisão.
- Random Forest.
- LightGBM.
- CatBoost.

Também foram testadas diferentes configurações de hiperparâmetros para a Árvore de Decisão, o Random Forest e o LightGBM.

### 4. Métrica de avaliação

A principal métrica utilizada foi o **RMSE (Root Mean Squared Error)**, ou raiz do erro quadrático médio.

Essa métrica mede a diferença entre os preços reais e os preços previstos pelo modelo. Como a variável-alvo é o preço do veículo, o RMSE é expresso na mesma unidade monetária dos dados, em euros.

**Quanto menor o RMSE, melhor o desempenho preditivo**, considerando os mesmos dados de avaliação.

## Resultados

### Comparação dos modelos na validação

Os resultados abaixo correspondem às configurações utilizadas na tabela final de comparação do notebook.

| Modelo | RMSE | Treinamento (s) | Previsão (s) |
|---|---:|---:|---:|
| LightGBM ajustado | 1.599,02 | 4,07 | 1,25 |
| Random Forest ajustado | 1.613,03 | 101,74 | 0,69 |
| CatBoost | 1.641,33 | 38,71 | 0,11 |
| Árvore de Decisão | 1.909,42 | 2,95 | 0,08 |
| Regressão Linear | 2.595,09 | 6,09 | 0,12 |

Os tempos são os registrados durante a execução do notebook e podem variar conforme o ambiente computacional.

### Principais observações

- **LightGBM:** apresentou o menor RMSE na validação, com 1.599,02, após o ajuste dos hiperparâmetros.
- **Random Forest:** alcançou um resultado próximo ao LightGBM, mas exigiu aproximadamente 101,74 segundos de treinamento.
- **CatBoost:** apresentou bom desempenho preditivo e tempo de previsão de aproximadamente 0,11 segundo.
- **Árvore de Decisão:** teve o menor tempo de treinamento e de previsão entre os modelos comparados, mas apresentou erros maiores.
- **Regressão Linear:** serviu como referência e apresentou o maior RMSE entre os modelos avaliados.

O LightGBM foi escolhido por combinar o menor erro de validação com um tempo de treinamento moderado, embora seu tempo de previsão tenha sido superior ao dos demais modelos.

## Avaliação final

Após a seleção dos hiperparâmetros, o LightGBM foi treinado novamente com os conjuntos de treinamento e validação combinados. Em seguida, foi avaliado no conjunto de teste, composto por dados não utilizados nas etapas anteriores.

| Métrica | Resultado |
|---|---:|
| RMSE de validação | 1.599,02 |
| RMSE de teste | 1.572,40 |
| Tempo de treinamento final | 5,21 segundos |
| Tempo de previsão final | 1,15 segundo |

O RMSE de teste foi próximo ao resultado obtido na validação e apresentou um valor menor, indicando que o desempenho se manteve consistente nos dados reservados para a avaliação final.

## Conclusão

O projeto demonstrou como diferentes algoritmos de regressão podem ser utilizados para estimar preços de carros usados, considerando não apenas a qualidade das previsões, mas também os custos computacionais.

Entre os modelos avaliados, o LightGBM ajustado apresentou o melhor resultado na validação, com RMSE de 1.599,02. Na avaliação final, alcançou RMSE de 1.572,40.

A comparação também evidenciou que o melhor modelo em qualidade preditiva nem sempre é o mais rápido para realizar previsões. A escolha depende do equilíbrio entre precisão, tempo de treinamento e velocidade de inferência exigido pela aplicação.

## Competências desenvolvidas

- Análise exploratória e limpeza de dados.
- Tratamento de valores ausentes, duplicados e inconsistentes.
- Engenharia e preparação de variáveis.
- Treinamento de modelos de regressão.
- Ajuste e comparação de hiperparâmetros.
- Avaliação de modelos com RMSE.
- Medição de tempo de treinamento e previsão.
- Seleção e avaliação final de modelos de Machine Learning.
