# Previsão de nota dos alunos
Muitos alunos de pós-graduação têm dificuldade em obter boas notas porque não recebem muito apoio nos cursos superiores em comparação com o apoio que os alunos recebem nas escolas. Alguns alunos precisam de muita atenção dos instrutores para que obtenham boas notas, sem isso, o estado emocional do aluno pode ser prejudicial para a sua carreira a longo prazo.

**O objetivo desse projeto é, através do aprendizado de máquina, prever as notas dos alunos para que os instrutores possam ajudar os alunos a se prepararem para tópicos em que as notas dos alunos foram previstas baixas.**

[https://www.kaggle.com/code/ramontanoeiro/student-performance/notebook](https://www.kaggle.com/code/ramontanoeiro/student-performance/notebook)
# #1 - Análise exploratória dos dados
Neste tópico buscamos entender melhor o conjunto de dados que temos em mãos com o objetivo de extrair informações que a base de dados fornece, compreender o fenômeno do target e, por fim, traçar estratégias de tratamento de dados que mais se adequam as características do dataset. A análise exploratória considerou as seguintes observações:
1. **Descrição dos atributos:** determinar se o atributo é categórico (ordinal/nominal) ou numérico (discreto/contínuo);
2. **Contagem de valores nulos:** importante para possíveis tratamentos de dados;
3. **Distribuição dos dados:** importante para determinar o tipo de normalização dos dados numéricos;
4. **Detecção de outliers:** importante para determinar o tipo de normalização dos dados numéricos. Dependendo da interpretação, podemos escolher uma normalização que desconsidera os outliers ou não;
5. **Contagem de valores distintos:** importante para determinar o tipo de tratamentos as variáveis categóricas iremos aplicar.
6. **Correlação dos dados:** entender dependências e associações dos atributos
7. **Multicolinearidade dos dados:** Importante para problemas de regressão, pois, podem reduzir a significância estatísticas das variáveis.
8. **Análise das informações pessoais dos alunos:** idade, gênero, endereço, saúde, localização...
9. **Análise familiar dos alunos:** tamanho da família, profissão dos pais, nível de escolaridade dos pais...
10. **Análise do suporte educacional dos alunos**  tempo de estudo, percurso até a escola, suporte familiar...
11. **Análise do relacionamento dos alunos:** relacionamento familiar, relacionamento amoroso, consumo de álcool...

# #2 - Tratamento dos dados
Neste tópico dividimos o tratamento dos dados nos seguintes quesitos:
|                                           | **Numéricos serão normalizados**                                     | **Numéricos não serão normalizados**                                     |
|-------------------------------------------|----------------------------------------------------------------------|--------------------------------------------------------------------------|
| **Ignorar multicolinearidade**            | Ignorar multicolinearidade e Numéricos serão normalizados            | Ignorar multicolinearidade e Numéricos não serão normalizados            |
| **Remover multicolinearidade alta**       | Remover multicolinearidade alta e Numéricos serão normalizados       | Remover multicolinearidade alta e Numéricos não serão normalizados       |
| **Remover multicolinearidade muito alta** | Remover multicolinearidade muito alta e Numéricos serão normalizados | Remover multicolinearidade muito alta e Numéricos não serão normalizados |

Dado que os algoritmos baseados em árvores de decisão não são baseados em distância, eles utilizarão a base de dados que não possuem numéricos normalizados.  
Ao utilizar o OneHotEncoder para tratar variáveis categóricas, neste tópico foi realizado o tratamento para evitar o *Dummy Variable Trap*, onde, as variáveis ​​independentes são multicolineares.

# #3 - Seleção de atributos
Neste tópico é utilizado o Recursive Feature Elimination (RFE), onde, o estimador é treinado no conjunto inicial de características e a importância de cada característica é obtida através de qualquer atributo específico ou chamável. Em seguida, os recursos menos importantes são removidos do conjunto atual de recursos. Esse procedimento é repetido recursivamente no conjunto podado até que o número desejado de características a serem selecionadas seja finalmente alcançado.  
**LIMITAÇÕES:** Os modelos: KNeighborsRegressor, MLPRegressor e SVR (exceto kernel linear) não possuem os atributos `coef_` ou `feature_importances_`. Com isso, não é possível realizar a seleção de atributos com esses algoritmos.
## Resultados
| Modelo        | Qtd Features | Base de dados                                          | Motivo                                                                                                                                |
|---------------|--------------|--------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| Random Forest | 5            | Atributos com multicolinearidade muito alta removidos  | Apesar de ser o 2º melhor desempenho entre os testes do RF, ele utiliza menos recursos e possui desvio padrão menor que o 1º colocado |
| XGBR          | 2            | Base de dados sem remoção de atributos multicolineares | 3 base de dados obteveram o mesmo melhor resultado                                                                                    |
| Decision Tree | 2            | Base de dados sem remoção de atributos multicolineares | 3 base de dados obteveram o mesmo melhor resultado                                                                                    |
| SVM Linear    | 1            | Base de dados sem remoção de atributos multicolineares | 3 base de dados obteveram o mesmo melhor resultado                                                                                    |
# #4 - Avaliação dos algoritmos
Neste tópico iremos realizar a hiperparametrização dos modelos. Os modelos utilizados nessa etapa são: KNN, MLP, SVM, Random Forest, Decision Tree e XGBooster.  
Após encontrar o conjunto de parâmetros ótimos para os modelos, foi realizado a etapa de Cross Validation para avaliar os algoritmos.

# #5 - Resultados finais
## SVM Linear
> Em média, o algoritmo SVM Linear erra a nota dos alunos em 1.21 pontos
MSE: 5.89 | RMSE: 2.43  

> Quando o modelo previu que o aluno pontuaria uma nota baixa, ele estava correto em 94.81% 
Quando o aluno pontuou nota baixa, o modelo conseguiu prever 98.65% dos casos

> Quando o modelo previu que o aluno pontuaria uma nota muito baixa (<=6), ele estava correto em 100.0%
Quando o aluno pontuou nota muito baixa (<=6), o modelo conseguiu prever 57.14% dos casos
## Decision Tree
> Em média, o algoritmo Decision Tree erra a nota dos alunos em 1.86 pontos
MSE: 8.44 | RMSE: 2.91

> Quando o modelo previu que o aluno pontuaria uma nota baixa, ele estava correto em 91.43%
Quando o aluno pontuou nota baixa, o modelo conseguiu prever 86.49% dos casos

> Quando o modelo previu que o aluno pontuaria uma nota muito baixa (<=6), ele estava correto em 100.0%
Quando o aluno pontuou nota muito baixa (<=6), o modelo conseguiu prever 50.0% dos casos