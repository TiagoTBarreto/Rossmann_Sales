# Previsão de Vendas Rossmann

<p align="center">
  <img src="https://github.com/TiagoTBarreto/Rossmann_Sales/assets/137197787/cef091c0-90cc-413c-83ed-ef9bb31c94c5" width="100%" height="300">
</p>



A companhia Rossmann foi fundada em 1972 é uma das maiores redes de drogarias da Europa, com cerca de 56.200 funcionários e mais de 4000 lojas espalhadas por diversos países. 

# 1. Problema de Negócio
O Chief Financial Officer (CFO) da Rossmann deseja reformar as lojas da rede de farmácias, visando melhorar a estrutura e o atendimento ao público. Para isso, ele informou aos gerentes que precisa receber a previsão de receita das próximas 6 semanas de cada loja, a fim de determinar o valor a ser investido em cada uma delas.

Atualmente, as previsões são feitas individualmente por cada gerente de loja, resultando em variações significativas devido a fatores distintos que influenciam os resultados, como promoções, competição por clientes, feriados e sazonalidade. O processo de cálculo é manual, o que torna os resultados ainda mais inconsistentes.

O objetivo deste projeto é auxiliar o CFO na tomada de decisões, fornecendo previsões automáticas para cada loja e permitindo que ele consulte essas previsões através de uma aplicação web no Streamlit.

# 2. Ferramentas Utilizadas

- Python 3.11.4: A linguagem de programação principal usada para desenvolver o projeto.
- Pyenv (Ambiente Virtual): Utilizado para isolar dependências e gerenciar versões do Python.
- Ferramentas Estatísticas.

**Biblioteca de Machine Learning e Otimização:**
- Scikit-learn: Empregado para a preparação de dados, treinamento de modelos, avaliação de desempenho e validação cruzada.
- Boruta: Utilizado para seleção de recursos.
- XGBoost: Implementação de algoritmo de aprendizado de máquina Gradient Boosting.
- Scikit-Optimize com BayesSearchCV: Utilizado para a busca de hiperparâmetros de forma eficiente.
  
**Desenvolvimento e Controle de Versão:**
- Git: Ferramenta de versionamento de código para rastrear alterações e colaboração em equipe.

**Implantação e Exposição do Modelo:**
- Flask: Usado para criar uma API RESTful, permitindo a hospedagem do modelo em produção.
- Streamlit WebApp: Criado para disponibilizar o acesso do modelo a qualquer usuário através da integração com a API do Flask.

**Habilidades e Abordagem:**
- Pensamento Crítico e Resolução de Problemas: Habilidades fundamentais aplicadas para analisar, solucionar problemas e tomar decisões ao longo do projeto.
  
# 3. Premissas assumidas para a análise
  - Em lojas onde a distância para o competidor mais próximo não estava disponível, foi assumido o valor de 200.000 metros.
  - Os dias em que as lojas estavam fechadas foram excluídos da análise.
  - A análise foi realizada apenas nos dias em que o número de vendas foi maior que zero.
  - A variável "clientes" foi removida da análise, pois não estaria disponível durante a previsão.

# 4. Descrição dos Dados 
- **Id** - an Id that represents a (Store, Date) duple within the test set
- **Store** - a unique Id for each store
- **Sales** - the turnover for any given day (this is what you are predicting)
- **Customers** - the number of customers on a given day
- **Open** - an indicator for whether the store was open: 0 = closed, 1 = open
- **StateHoliday** - indicates a state holiday. Normally all stores, with few exceptions, are closed on state holidays. Note that all schools are closed on public holidays and weekends. a = public holiday, b = Easter holiday, c = Christmas, 0 = None
- **SchoolHoliday** - indicates if the (Store, Date) was affected by the closure of public schools
- **StoreType** - differentiates between 4 different store models: a, b, c, d
- **Assortment** - describes an assortment level: a = basic, b = extra, c = extended
- **CompetitionDistance** - distance in meters to the nearest competitor store
- **CompetitionOpenSince[Month/Year]** - gives the approximate year and month of the time the nearest competitor was opened
- **Promo** - indicates whether a store is running a promo on that day
- **Promo2** - Promo2 is a continuing and consecutive promotion for some stores: 0 = store is not participating, 1 = store is participating
- **Promo2Since[Year/Week]** - describes the year and calendar week when the store started participating in Promo2
- **PromoInterval** - describes the consecutive intervals Promo2 is started, naming the months the promotion is started anew. E.g. "Feb,May,Aug,Nov" means each round starts in February, May, August, November of any given year for that store
  
# 5. Descrição da solução
Foi empregado o método de gerenciamento CRIPS-DM, que tem como objetivo o desenvolvimento de projetos de Data Science de forma cíclica. Esse método é abrangente e, ao concluir um ciclo, você obterá:
- Uma versão completa da solução.
- Maior rapidez na entrega de valor.
- Mapeamento de todos os possíveis problemas.

O CRIPS-DS é composto pelos seguintes passos: 
![image](https://github.com/TiagoTBarreto/Rossmann_Sales/assets/137197787/f4cac96f-a228-4e28-b5a2-eb16f29d5a39)

## 1.0 Questão de Negócio:
Os gerentes das lojas solicitaram a previsão de vendas para as próximas seis semanas.

## 2.0 Entendimento do Negócio:
Normalmente, um Cientista de Dados recebe uma solução para um problema em vez do próprio problema em si. Portanto, antes de iniciar o projeto, é essencial responder a quatro perguntas fundamentais: Qual é a motivação por trás do problema? Qual é a causa raiz do problema? Quem é o StakeHolder do problema? Qual é o formato de entrega da solução desejada? Com base nas respostas a essas perguntas, pode-se determinar se a previsão é realmente a melhor abordagem. Após analisar as respostas, concluí que a previsão de vendas era, de fato, a melhor opção, e assim iniciei o projeto.

## 3.0 Coleta dos Dados:
Em projetos de Ciência de Dados em empresas, é comum o uso da linguagem SQL e solicitações em API para extrair as tabelas de dados necessárias. No entanto, neste projeto pessoal, os dados utilizados são públicos e foram baixados do Kaggle.

## 4.0 Filtragem dos Dados:
Nesta etapa, realizamos uma análise descritiva dos dados, aplicamos engenharia de características e filtramos linhas e colunas de acordo com as restrições de negócios. Também elaboramos um mapa mental que resume essa abordagem.

![image](https://github.com/TiagoTBarreto/Rossmann_Sales/assets/137197787/d20b18b8-842b-4bfb-842e-d2c226da4080)

Com base nesse mapa mental, formulei inicialmente 19 hipóteses. No entanto, devido às limitações das características disponíveis, optei por validar apenas 11 dessas hipóteses.

## 5.0 Exploração dos Dados:
Foram realizadas análises de dados de três tipos: univariadas, bivariadas e multivariadas. Essas análises têm o objetivo de nos ajudar a compreender melhor o conjunto de dados e identificar quais características podem ser relevantes. Também foi usado essas análises para validar as hipóteses que foram formuladas anteriormente.

## 6.0 Preparação dos Dados:
A maioria dos modelos de Machine Learning foram criados com a premissa de que os dados estão na mesma escala numérica e seguem uma distribuição normal. Portanto, nesta etapa, realizei a preparação dos dados para otimizar o aprendizado do modelo. Isso inclui:

**A técnica de normalização não foi aplicada, uma vez que nenhuma das variáveis apresentava uma distribuição normal.**
- Reescalonamento de variáveis.
- Codificação de variáveis categóricas.
- Transformação da variável de resposta e aplicação de transformações cíclicas.
  
O conjunto de dados foi dividido em um conjunto de treinamento, enquanto as últimas 6 semanas foram reservadas para fins de teste, a fim de simular a situação real de prever as próximas 6 semanas. Em seguida, utilizou-se o algoritmo Boruta para selecionar as variáveis mais relevantes para o modelo, seguindo o princípio da Navalha de Occam. Esse princípio afirma que um modelo mais simples tende a generalizar melhor do que um modelo complexo, portanto, procuramos reduzir o número de variáveis, mantendo ao mesmo tempo o desempenho máximo.

## 7.0 Algoritmos de Machine Learning: 
### 7.1 Algoritmos
Foram selecionados 5 algoritmos:
  1. Average Model
  2. Linear Regression
  3. Linear Regression Regularized
  4. Random Forest Regressor
  5. XGBoost

### 7.2 Performance dos Algoritmos sem Cross-Validation:
**Os algoritmos foram avaliados com base na métrica RMSE**

Modelo Médio (Average Model) foi usado como linha de base para permitir a comparação de desempenho com outros algoritmos. Inicialmente, foram testados algoritmos lineares e mais simples para entender a complexidade do problema. Como esses algoritmos não apresentaram um bom desempenho, posteriormente foram testados o Random Forest e o XGBoost, que são algoritmos mais complexos. Após essa análise preliminar da complexidade do problema, obtivemos os seguintes resultados:

![image](https://github.com/TiagoTBarreto/Rossmann_Sales/assets/137197787/f5b31ce7-8fa0-4ebd-94b6-e625990864d2)

### 7.3 Performance dos Algoritmos com Cross-Validation:
![image](https://github.com/TiagoTBarreto/Rossmann_Sales/assets/137197787/1998fc18-93da-49f6-8625-90fcf3b350dc)

Em seguida, foi aplicado o método de Validação Cruzada TimeSeriesSplit com 5 divisões (splits) para garantir a avaliação real do desempenho do modelo. Após os testes, obtivemos os seguintes resultados:

![image](https://github.com/TiagoTBarreto/Rossmann_Sales/assets/137197787/b405e357-dd67-4a14-a474-bd7a5b22a196)

### 7.4 Escolha do Algoritmo:
**Embora a Random Forest tenha apresentado um desempenho superior, optei pelo XGBoost para a próxima fase devido aos seguintes motivos:** 
- Requer significativamente menos armazenamento.
- Apresenta um processamento consideravelmente mais rápido
- Oferece economia em termos de custos computacionais.

### 7.5 HyperParameter Fine Tunning:
Esta fase tem como objetivo encontrar os melhores parâmetros para o modelo. Foi utilizado o método Bayesian Search, que não procura os melhores parâmetros aleatoriamente, mas aprende a cada iteração, buscando os parâmetros que minimizam o erro ao máximo. Devido à capacidade de armazenamento limitada na nuvem, este foi o espaço de busca definido:
- 'max_depth': (5, 15)   
- 'learning_rate': (0.03, 0.15, 'log-uniform')
- 'subsample': (0.3, 0.7, 'uniform')
- 'n_estimators': (300, 1000)

O Bayesian Search identificou os seguintes como os melhores parâmetros dentro do espaço de busca:
- 'learning_rate': 0.0509 
- 'max_depth': 10
- 'n_estimators': 861
- 'subsample': 0.5051
  
Apesar de esses terem sido os melhores parâmetros, por questões de armazenamento, reduziu-se o valor de 'max_depth' para 9 e 'n_estimators' para 375.
### 7.6 Performance Final
![image](https://github.com/TiagoTBarreto/Rossmann_Sales/assets/137197787/c89fdb88-06db-4810-a497-485499a1c2ce)

## 8.0 Avaliação do Algoritmo:
Este estágio do projeto é um ponto crucial, pois transita da parte técnica, envolvendo modelos e algoritmos, para a tradução e interpretação do erro do modelo em termos de negócio. Ele fornece uma estimativa de quanto esse modelo contribuirá para o aumento da receita da empresa. Nesta etapa, é possível realizar uma comparação com o estado atual do negócio (status quo), mas, como não dispomos de informações sobre o desempenho dos métodos de previsão utilizados por cada gerente, basearemos o desempenho do modelo no aumento da receita por meio dos cenários de vendas projetados.
### 8.1 Performance do Modelo na Visão de Negócio
**Foi utilizada a métrica MAE para avaliação do modelo em questão do desempenho de negócio**

![image](https://github.com/TiagoTBarreto/Rossmann_Sales/assets/137197787/7645d1e2-b531-4158-932e-fc6bfd2760e8)
![image](https://github.com/TiagoTBarreto/Rossmann_Sales/assets/137197787/369ee438-08e6-4cfa-8158-b84ac9fdace6)

- O modelo teve um erro médio de 10%, apresentando um desempenho sólido na maioria das lojas, mas um desempenho ruim em duas delas.
- No primeiro gráfico, é possível observar a distribuição das lojas em relação ao erro médio. Nota-se uma grande concentração de pontos em uma região de baixo erro, com alguns pontos dispersos nas regiões de maior erro.
- No segundo gráfico, é possível observar o número de lojas distribuídas em faixas de erro. Podemos notar que mais de 1000 das 1115 lojas têm um erro abaixo de 15%, enquanto apenas 4 lojas têm um erro acima de 30%.

### 8.2 Performance Total do Modelo
O modelo previu vendas de 282 milhões para as próximas 6 semanas. Devido ao seu erro médio de 10%, ele pode, na pior das hipóteses, subestimar o valor em 10% ou superestimar em 10%, como pode ser observado no dataframe a seguir, que apresenta o número de vendas previsto, o melhor cenário e o pior cenário.

![image](https://github.com/TiagoTBarreto/Rossmann_Sales/assets/137197787/75cb0041-fb29-4244-911e-255b4b2799ce)

### 8.3 Performance do Modelo na Visão de Aprendizado de Máquina
**Análise de Desempenho das Previsões do Modelo**
![image](https://github.com/TiagoTBarreto/Rossmann_Sales/assets/137197787/248bf8b0-8626-4bf4-91e3-240d19172013)

- No primeiro gráfico, é possível observar que ao longo do tempo, embora as linhas de vendas reais e vendas previstas sejam um pouco distintas, elas apresentam um comportamento muito semelhante.
- No segundo gráfico, o eixo y representa a 'error_rate' (taxa de erro). O cenário ideal é representado pela linha tracejada que corresponde a uma taxa de 1, indicando que o modelo não cometeu erros em suas previsões. Através desse gráfico, é possível determinar se o modelo, ao longo do tempo, está subestimando ou superestimando o número de vendas. Se a taxa estiver abaixo de 1, o modelo está subestimando; se estiver acima de 1, o modelo está superestimando.



# 4. Top 2 Insights
## 1.0 Lojas com mais promoções consecutivas vendem em média menos.
![image](https://github.com/TiagoTBarreto/Rossmann_Sales/assets/137197787/6cd3c587-2132-466c-b232-48e6c2c0c533)

## 2.0 Lojas com competidores mais próximos vendem em média mais.
![image](https://github.com/TiagoTBarreto/Rossmann_Sales/assets/137197787/af62eb8f-6d2e-43d9-ac95-004e6a97154e)

# 5. Machine Learning
- Average Model
- Linear Regression
- Linear Regression Regularized
- Random Forest Regressor
- XGBoost Regressor

# 7. O produto final do projeto
WebApp online, hospedado no Streamlit Cloud e integrado com o modelo que está no Render, está disponível para acesso em qualquer dispositivo conectado à internet, possibilitando que qualquer consumidor tenha acesso ao modelo. Você pode acessar o WebApp através do seguinte link: https://rossmann-sales-forecast.streamlit.app/

![image](https://github.com/TiagoTBarreto/Rossmann_Sales/assets/137197787/c8540192-01d1-41df-9b1a-1bef31cba492)
**A imagem acima é do WebApp, como pode se observar na barra lateral tem 3 filtros:**
- ID Store: Este filtro controla o número das lojas para as quais a previsão será realizada.
- Sales: Este filtro estabelece um limite de vendas para as lojas que serão exibidas posteriormente no dataframe.
- Budget: Dado que a ideia do CFO é alocar uma parte da receita para reformas, este filtro permite a customização da porcentagem das vendas que será destinada a esse propósito.

**Após a previsão, um dataframe é gerado e pode ser baixado ao clicar no botão "DOWNLOAD CSV", permitindo assim a manipulação desse dataframe de acordo com as necessidades do CFO.**
  
# 8. Conclusão
- O projeto fornece uma solução automatizada para a previsão de vendas das lojas da Rossmann, eliminando a necessidade de previsões manuais feitas por gerentes de loja.
- O modelo de previsão de vendas desenvolvido demonstrou um desempenho consistente na maioria das lojas, com um erro médio de aproximadamente 10%. No entanto, é importante observar que o desempenho pode variar entre as lojas. Portanto, em primeiro lugar, podemos utilizar como referência para o orçamento de reformas as mais de 600 lojas com erro inferior a 10%. Dependendo do desempenho atual do método utilizado para a previsão de vendas, podemos considerar a inclusão das previsões das lojas com erro até 15% ou 20%. No entanto, aquelas que apresentarem um erro superior a esse valor deveriam ser discutidas com o CFO, e não devemos considerar as previsões das lojas 292 e 909, que possuem erros superiores a 50%.
- Uma das principais descobertas foi que as lojas que realizam promoções consecutivas tendem a vender em média menos. Isso pode ser útil para o CFO ao tomar decisões sobre a alocação de recursos para promoções.
- Outra descoberta importante foi que as lojas com competidores mais próximos tendem a vender em média mais. Isso pode ser uma informação valiosa ao considerar a localização das lojas e a concorrência.

# 9. Próximo passos
Se fosse continuar o trabalho nesse projeto, realizando um segundo ciclo do CRISP-DS, consideraria os seguintes passos para tentar criar um novo modelo para as lojas com baixo desempenho ou melhorar o desempenho geral do modelo atual, sem outliers com grandes erros:
- Conduzir uma análise aprofundada para identificar as particularidades das lojas com baixo desempenho que estão dificultando a precisão das previsões do modelo.
- Coletar mais Dados.
- Efetuar a criação de novas variáveis a partir do conjunto de dados já existente.
- Experimentar diferentes modelos de Machine Learning.
- Formulação de novas hipóteses para gerar novos insights para o negócio.
