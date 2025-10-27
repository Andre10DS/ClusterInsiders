# Insiders Clustering

## Este projeto tem como objetivo descobrir pessoas semelhantes para participar de um programa de fidelidade


# 1. Business Problem.

A empresa All in One Place é uma empresa de outlet multimarcas, ou seja, comercializa produtos de segunda linha de várias marcas a um preço menor através de um e-commerce. 

Ao analisar a base de clientes o time de marketing constatou a existência de alguns clientes que compram produtos mais caros e com alta frequência contribuindo com uma parcela significativa do faturamento. Baseado nessa percepção, o time de marketing vai lançar um programa de fidelidade para os melhores clientes da base. chamado insiders. Mas o time não tem conhecimento avançado em análise de dados para eleger os participantes do programa.

Por esse motivo, o time de marketing requisitou ao time de dados uma seleção de clientes elegiveis ao programa, usando técnicas avançadas de manipulação de dados.

# 2. Premissas de negócio.

1. No conjunto de características utilizadas para agrupar os grupos de clientes deverá conter pelo menos a receita e frequencia de compra.
2. O resultado deverá ser entregue em uma apresentação com as dúvidas de negócio.
3. Deverá ser entregue um dashboard em looker para acompanhamento dos clientes insiders e demais grupos para que seja possível realizar o acompanhamento.

# 3. Objetivo.

1. Deve ser entregue a lista de clientes que serão elegíveis ao grupo de insiders.
2. Responder o conjunto das perguntas de negócio sobre o grupo insiders.
3. A descrição do grupo, a lista de clientes e as respostas das questões de negócio deverão ser entregues em uma apresentação e deverá ser criado um dashboard para acompanhamento do grupo.

# 3. Planejamento da solução.

**Step 01. Descrição dos dados:**

  - Ajustar o nome das colunas.
  - Checar os tipos de dados e verificar a necessidade de alteração.
  - Checar os NA e realizar e o replace caso necessário.
  - Relizar a descrição estatisticas dos dados númericos e avaliar distorções.

**Step 02. Filtragem de Variáveis:**
 
  - Filtrar todas as colunas e dados que não irão contribuir para construção do projeto ou que sofre algum restrição de atualização. Seguindo as premissas anteriores foram filtrados os seguintes dados:
    
    1. Coluna unit_price para valores maiores ou iguais a 0.04 para evitar a entrada de dados negativos que podem ser realacionados a descontos.
    2. Na coluna stock_code foram retirados os códigos 'POST', 'D', 'DOT', 'M', 'S', 'AMAZONFEE', 'm', 'DCGSSBOY', 'DCGSSGIRL', 'PADS', 'B' e 'CRUK' que podem conter diversas características não relacionados a venda.
    3. Foi excluído a coluna description por não ter uma relevancia para a construção do modelo.
    4. Foram retirados os itens 'European Community' e 'Unspecified' da coluna country por não terem um divisão adequada do país ou por não terem uma descrição clara.
    5. Foi retirado o código 16446 da coluna 'customer_id' por apresentar um valor muito distoante dos demais.
    6. A base foi dividida em duas sendo uma chamada de returns ( base de devolução com valores negativos) e purchases (representa a base de vendas)

**Step 03. Feature Engineering:**

  - O processo de feature engineering visa a criação de features a partir da combinação de features ou da mudança de formatos. Essas novas features podem ajudar a melhorar o resultado do modelo criando uma maior variabilidade dos dados. Segue o conjunto de features criadas:

    1. Gross Revenue: Faturamento por cliente
    2. Recency : Tem entra última compra.
    3. Quantity of purchased: Quantidade de compra
    4. Quantity total of items purchased: Quantidade total de itens por compra
    5. Quantity of products purchased: Quantidade de tipos diferentes de itens comprados.
    6. Average Ticket Value: Valor médio por compra.
    7. Average Recency Days: Tempo médio das recências
    8. Frequency Purchase: Frequencia de compra.
    9. Number of Returns: Quantidade de devoluções
    10. Basket Size: Tamanho da cesta.
    11. Unique Basket Size: Quantidade de produtos distintos por cesta.
 

**Step 04. Exploração dos dados e analise dos espaços:**

    - Nesta etapa foram realizados a avaliação univariada utilizando o pandas profile para avaliar as metricas de tendencias central e de dispersão. Além disso, foi avaliado a distribuição dos dados de cada features para avaliar o formato da curtose, assimetria e exitencia de outlier.
    - Além da univariada, foi realizado a analise bivariada visando encontrar alguma alta correlação que pode reduzir a perfomance do modelo.

**Step 05. Estudo do espaço:**
    - Nessa etapa foi avaliado a distribuição dos dados em menor dimensionaldiade. Para reduzir a dimensionalidade usamos o PCA, UMAP, TSNE e Baseado em Embedding.

    1. No espaço dos componentes principais gerado pelo PCA a distribuição do dados não apresentou uma separabilidade, conforme figura abaixo:
     *** Colocar imagem ***
    2. No espaço gerado pelo UMAP a distribuição teve uma evolução se comprado ao PCA, porém, ainda apresenta uma concentração em determinadas regiões, conforme figura abaixo:
     *** Colocar imagem ***
    3. No espaço gerado pelo TSNE a distrbuição teve uma maior separabilidade se comparado ao PCA, porém ainda apresenta baixa separabilidade dos dados.
     *** Colocar imagem ***
    4. O espaço gerado Random Forest (Embedding) apresentou a melhor distribuição dos dados com a formação de clusters mais evidentes.
    *** Colocar imagem ***


**Step 06. Hyperparameter Fine-Tunning e teste dos modelos:**
    
   - Nesta etapa foi realizar o teste com os modelos e ajuste dos parametros com os modelos K-Means, GMM, Hierarchical Clustering e DBSCAN. Os resultados obtidos foram:


      *** foto da perfomance ***

   Foi tomado a decisão de escolher o número de 8 clusters para facilitar o direcionamento das ações de marketing. Além disso, o modelo selecionado foi o Hierarchical Clustering por apresentar a melhor perfomance para o número de clusters escolhido.


**Step 08. Analise do clusters:**

   - Ao definir os clusters obtivemos as seguintes descrições:

**Step 09. Top 3 Data Insights:**

H1: Os clientes do cluster insiders possuem um volume (produtos) de compras acima de 10% do total de compras.

Verdade: O cluster insider possuem um volume de compra de produtos de 48.69%



H2: Os clientes do cluster insiders possuem um volume (faturamento) de compras acima de 10% do total de compras

Verdadeiro: O cluster insider possuem um volume de GMV de 49.01%



H3: Os clientes do cluster insiders tem um número de devolução médio abaixo da média da base total de clientes

Falso: O cluser insiders tem a média de devoluções acima da média geral


**Step 09. Questões de negócio :**

1. Quem são as pessoas elegíveis para participar do programa de Insiders ?

Respostas: 

2. Quantos clientes farão parte do grupo?

Respostas:

3. Quais as principais características desses clientes ?
Respostas:

4. Qual a porcentagem de contribuição do faturamento, vinda do Insiders ?

Respostas:

5. Quais as condições para uma pessoa ser elegível ao Insiders ?

Respostas:

6. Quais as condições para uma pessoa ser removida do Insiders ?

Respostas:


7. Qual a garantia que o programa Insiders é melhor que o restante da base ?

Respostas:

8. Quais ações o time de marketing pode realizar para aumentar o faturamento?

Respostas:


**Step 10. Deploy Modelo to Production:**

# 8. Conclusions

  Os algoritmos de clusterização são ferramentas extremamente úteis para entender o comportamento do cliente e na resolução de problemas de negócio que necessitem de agrupamentos se rotulos pré-definidos. Neste projeto a utilização dos algoritmos ajudaram a definir um grupo de clientes para criação do programa de fidelização e, além disso, contribuição para a criação de um grupo de referência para nortear as ações de marketing visando aumentar o desempenho da empresa transformando os demais grupos em insiders.


# 9. Next Steps to Improve

  - Realizar o estudo da cesta de compras do grupo insiders para subsidear as ações de marketing para outros grupos com o objetivo de aproxima-los do padrão mais elevado.
  - Efetuar a projeção de faturamento do grupo insiders para avaliar a viabilidade das ações de marketing.


