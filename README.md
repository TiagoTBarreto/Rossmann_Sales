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

**Ferramentas para Análise de Dados**
- Python 3.11.4: A linguagem de programação principal usada para desenvolver o projeto.
- Ferramentas Estatísticas para Análise dos Dados.

**Biblioteca de Machine Learning e Otimização:**
- Scikit-learn: Empregado para a preparação de dados, treinamento de modelos, avaliação de desempenho e validação cruzada.
- Boruta: Utilizado para seleção de recursos.
- XGBoost: Implementação de algoritmo de aprendizado de máquina Gradient Boosting.
- Scikit-Optimize com BayesSearchCV: Utilizado para a busca de hiperparâmetros de forma eficiente.
  
**Desenvolvimento e Controle de Versão:**
- Git: Ferramenta de versionamento de código para rastrear alterações e colaboração em equipe.
- Pyenv (Ambiente Virtual): Utilizado para isolar dependências e gerenciar versões do Python.

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
| Campo                       | Descrição                                                                                      |
|-----------------------------|-------------------------------------------------------------------------------------------------|
| Id                          | Um ID que representa um par (Loja, Data) no conjunto de teste.                                  |
| Store                       | Um ID exclusivo para cada loja.                                                                |
| Sales                       | O faturamento para um determinado dia (o que você está prevendo).                                |
| Customers                   | O número de clientes em um determinado dia.                                                  |
| Open                        | Um indicador de se a loja estava aberta: 0 = fechada, 1 = aberta.                                  |
| StateHoliday                | Indica um feriado estadual. Normalmente, todas as lojas, com poucas exceções, estão fechadas em feriados estaduais. Observe que todas as escolas estão fechadas em feriados públicos e fins de semana. a = feriado público, b = feriado de Páscoa, c = Natal, 0 = Nenhum. |
| SchoolHoliday               | Indica se o (Loja, Data) foi afetado pelo fechamento das escolas públicas.                       |
| StoreType                   | Diferencia entre 4 modelos diferentes de loja: a, b, c, d.                                       |
| Assortment                  | Descreve um nível de sortimento: a = básico, b = extra, c = estendido.                              |
| CompetitionDistance         | Distância em metros para a loja concorrente mais próxima.                                       |
| CompetitionOpenSince[Month/Year] | Dá o ano e mês aproximados em que o concorrente mais próximo foi aberto.                        |
| Promo                       | Indica se uma loja está executando uma promoção naquele dia.                                     |
| Promo2                      | Promo2 é uma promoção contínua e consecutiva para algumas lojas: 0 = loja não está participando, 1 = loja está participando.                                       |
| Promo2Since[Year/Week]      | Descreve o ano e a semana do calendário em que a loja começou a participar da Promo2.             |
| PromoInterval               | Descreve os intervalos consecutivos em que a Promo2 é iniciada, nomeando os meses em que a promoção é reiniciada. Por exemplo, "Fev, Mai, Ago, Nov" significa que cada rodada começa em fevereiro, maio, agosto, novembro de qualquer ano para essa loja.             |

  
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

| Model Name              | MAE CV              | MAPE CV           | RMSE CV             |
|-------------------------|---------------------|-------------------|---------------------|
| Random Forest Regressor | 983.99 +/- 196.91  | 0.14 +/- 0.02     | 1569.12 +/- 361.45  |
| XGBoost Regressor       | 1133.19 +/- 177.0  | 0.17 +/- 0.02     | 1645.36 +/- 285.61  |
| Average Model           | 1354.80         | 0.20            | 1835.13         |
| Lasso                   | 2248.0 +/- 80.6    | 0.37 +/- 0.01     | 3133.63 +/- 150.18  |
| Linear Regression       | 2315.4 +/- 630.73  | 0.34 +/- 0.06     | 3222.41 +/- 694.42  |


**Após uma análise das métricas com Cross-Validation de 5 splits a Random Forest apresentou o melhor desempenho, mas optei pelo XGBoost para a próxima fase pelos seguintes motivos:** 
- Requer significativamente menos armazenamento.
- Apresenta um processamento consideravelmente mais rápido
- Oferece economia em termos de custos computacionais.

**Modelo Final:**
| Model Name              | MAE                | MAPE              | RMSE               |
|-------------------------|--------------------|-------------------|---------------------|
| XGBoost Regressor       | 708.99         | 0.1027          | 1037.78         |


# 8. Tradução do modelo de Machine Learning
O modelo previu vendas de R$ 282 milhões para as próximas 6 semanas. Devido ao seu erro médio de 10%, ele pode, na pior das hipóteses, subestimar o valor em 10% ou superestimar em 10%, como pode ser observado abaixo. Se comparado com o modelo baseline de média que tem um erro de 20%, ele diminui em 10% o erro das predições atuais.

| Cenário      | Vendas         |
|----------------|-----------------|
| Predição    | R$ 282.697.408,00  |
| Pior Cenário  | R$ 253.204.264,64  |
| Melhor Cenário   | R$ 312.190.531,69  |


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
Se fosse continuar o trabalho nesse projeto, realizando um segundo ciclo do CRISP-DM, consideraria os seguintes passos para tentar criar um novo modelo para as lojas com baixo desempenho ou melhorar o desempenho geral do modelo atual, sem outliers com grandes erros:
- Conduzir uma análise aprofundada para identificar as particularidades das lojas com baixo desempenho que estão dificultando a precisão das previsões do modelo.
- Coletar mais Dados.
- Efetuar a criação de novas variáveis a partir do conjunto de dados já existente.
- Experimentar diferentes modelos de Machine Learning.
- Formulação de novas hipóteses para gerar novos insights para o negócio.
