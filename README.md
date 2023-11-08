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

O CRIPS-DM é composto pelos seguintes passos: 
![image](https://github.com/TiagoTBarreto/Rossmann_Sales/assets/137197787/f4cac96f-a228-4e28-b5a2-eb16f29d5a39)

# 6. Top 2 Insights
## 1.0 Lojas com mais promoções consecutivas vendem em média menos.
![image](https://github.com/TiagoTBarreto/Rossmann_Sales/assets/137197787/6cd3c587-2132-466c-b232-48e6c2c0c533)

## 2.0 Lojas com competidores mais próximos vendem em média mais.
![image](https://github.com/TiagoTBarreto/Rossmann_Sales/assets/137197787/af62eb8f-6d2e-43d9-ac95-004e6a97154e)

# 7. Machine Learning
- Average Model
- Linear Regression
- Linear Regression Regularized
- Random Forest Regressor
- XGBoost Regressor

![image](https://github.com/TiagoTBarreto/Rossmann_Sales/assets/137197787/b5b0fe72-2045-407c-a875-ddad43dc766d)

**Após uma análise das métricas com Cross-Validation de 5 splits a Random Forest apresentou o melhor desempenho, mas optei pelo XGBoost para a próxima fase pelos seguintes motivos:** 
- Requer significativamente menos armazenamento.
- Apresenta um processamento consideravelmente mais rápido
- Oferece economia em termos de custos computacionais.

# 8. Tradução do modelo de Machine Learning
O modelo previu vendas de R$ 282 milhões para as próximas 6 semanas. Devido ao seu erro médio de 10%, ele pode, na pior das hipóteses, subestimar o valor em 10% ou superestimar em 10%, como pode ser observado no dataframe a seguir, que apresenta o número de vendas previsto, o melhor cenário e o pior cenário.

![image](https://github.com/TiagoTBarreto/Rossmann_Sales/assets/137197787/75cb0041-fb29-4244-911e-255b4b2799ce)

# 9. O produto final do projeto
WebApp online, hospedado no Streamlit Cloud e integrado com o modelo que está no Render, está disponível para acesso em qualquer dispositivo conectado à internet, possibilitando que qualquer consumidor tenha acesso ao modelo. Você pode acessar o WebApp através do seguinte link: https://rossmann-sales-forecast.streamlit.app/

![image](https://github.com/TiagoTBarreto/Rossmann_Sales/assets/137197787/c8540192-01d1-41df-9b1a-1bef31cba492)
**A imagem acima é do WebApp, como pode se observar na barra lateral tem 3 filtros:**
- ID Store: Este filtro controla o número das lojas para as quais a previsão será realizada.
- Sales: Este filtro estabelece um limite de vendas para as lojas que serão exibidas posteriormente no dataframe.
- Budget: Dado que a ideia do CFO é alocar uma parte da receita para reformas, este filtro permite a customização da porcentagem das vendas que será destinada a esse propósito.

**Após a previsão, um dataframe é gerado e pode ser baixado ao clicar no botão "DOWNLOAD CSV", permitindo assim a manipulação desse dataframe de acordo com as necessidades do CFO.**
  
# 10. Conclusão
- O projeto fornece uma solução automatizada para a previsão de vendas das lojas da Rossmann, eliminando a necessidade de previsões manuais feitas por gerentes de loja.
- O modelo de previsão de vendas desenvolvido demonstrou um desempenho consistente na maioria das lojas, com um erro médio de aproximadamente 10%. No entanto, é importante observar que o desempenho pode variar entre as lojas. Portanto, em primeiro lugar, podemos utilizar como referência para o orçamento de reformas as mais de 600 lojas com erro inferior a 10%. Dependendo do desempenho atual do método utilizado para a previsão de vendas, podemos considerar a inclusão das previsões das lojas com erro até 15% ou 20%. No entanto, aquelas que apresentarem um erro superior a esse valor deveriam ser discutidas com o CFO, e não devemos considerar as previsões das lojas 292 e 909, que possuem erros superiores a 50%.
- Uma das principais descobertas foi que as lojas que realizam promoções consecutivas tendem a vender em média menos. Isso pode ser útil para o CFO ao tomar decisões sobre a alocação de recursos para promoções.
- Outra descoberta importante foi que as lojas com competidores mais próximos tendem a vender em média mais. Isso pode ser uma informação valiosa ao considerar a localização das lojas e a concorrência.

# 11. Próximo passos
Se fosse continuar o trabalho nesse projeto, realizando um segundo ciclo do CRISP-DS, consideraria os seguintes passos para tentar criar um novo modelo para as lojas com baixo desempenho ou melhorar o desempenho geral do modelo atual, sem outliers com grandes erros:
- Conduzir uma análise aprofundada para identificar as particularidades das lojas com baixo desempenho que estão dificultando a precisão das previsões do modelo.
- Coletar mais Dados.
- Efetuar a criação de novas variáveis a partir do conjunto de dados já existente.
- Experimentar diferentes modelos de Machine Learning.
- Formulação de novas hipóteses para gerar novos insights para o negócio.
