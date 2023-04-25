# Dashboards em PowerBI

> Aqui se encontram dashboards construídos a partir dos laboratórios hands-on do curso 'Microsoft Power BI Para Business Intelligence e Data Science' da DSA.

**OBS.**: Como o foco inicialmente foi em como utilizar os recursos, a aparência dos primeiros Dashboards foi bem simplificada.

No curso foi abordado alguns temas relevantes como:
1. Introdução à ferramenta, Análise de Dados, BI.
2. Modelagem de dados, relacionamentos e introdução à linguagem DAX;
3. Grande quantidade de laboratórios hands-on com diferentes cenários e de ramos diferentes como marketing, RH, logística.

## 1 - Capítulo 1 - Análise Global de Vendas
Acesse ao primeiro dash pelo [link](https://app.powerbi.com/view?r=eyJrIjoiM2E3ODAzODUtYzBmOS00ZDIyLWI2NTctMjZkMTRjZTM4ZDFlIiwidCI6Ijg4YjY0MTgxLWQ5NGQtNGU4ZC1hNjNlLTIyZDg3ZWU5NDFkYyJ9)

Buscava-se responder às seguintes perguntas:
1. Qual o valor total vendido?
2. Quantas vendas foram realizadas por categoria de produto?
3. Quantas vendas foram realizadas por país considerando a prioridade de entrega?
4. Qual foi a média de desconto nas vendas por subcategoria de produto?
5. Quais países tiveram maior média de valor de venda? Demonstre em um mapa.

E nosso Dashboard deve dar ao usuário a possibilidade de filtrar os dados por ano, por segmento e por país.

## Capítulo 2 - Relacionamentos, DAX e Modelagem
Ao decorrer desse capítulo além da produção de mais um dashboard, foram vistos introdução à linguagem DAX e pontos sobre Relacionamentos e Modelagem de dados no PBI.

Acesse ao dash pelo [link](https://app.powerbi.com/view?r=eyJrIjoiNTFlZGQ4ZTUtNGNkZi00MmUzLTllNjUtZTdmYTZiZjhiNGZiIiwidCI6Ijg4YjY0MTgxLWQ5NGQtNGU4ZC1hNjNlLTIyZDg3ZWU5NDFkYyJ9)

Buscava-se responder às seguintes perguntas:
1. Qual foi o total de valor venda considerando cada modo de envio dos pedidos? Use um gráfico de cascata.
2. Quais mercados tiveram o maior custo médio de envio dos produtos vendidos? Use um gráfico treemap.
3. A empresa tem como objetivo (meta) manter uma média de 350 para o valor de venda todos os meses. Mostre um indicador (KPI–Key Performance Indicator) com o valor médio de venda. A empresa ficou abaixo ou acima da meta no mês de Abril/2014?
4. Considere que o lucro é equivalente a:valor venda -custo envio. Qual categoria de produto apresentou maior lucro médio.
5. Qual foi o comportamento da margem de lucro ao longo do tempo? Considere amargem de lucro como o lucro dividido pelo valor venda.

## Capítulo 3 - Análise de Campanhas de Marketing
Durante a construção desse Dashboard foi visto novos gráficos e seu funcionamento - como o gráfico de barras e linhas. A criação de várias visões de uma campanha de marketing e como acompanhar os mais importantes KPIs.

Acesse ao dash pelo [link](https://app.powerbi.com/view?r=eyJrIjoiZmE0ZWQxNDgtMWNiOC00N2Q2LWIwZWYtOGVhNTAwNDNiNTM1IiwidCI6Ijg4YjY0MTgxLWQ5NGQtNGU4ZC1hNjNlLTIyZDg3ZWU5NDFkYyJ9).

A construção desse relatório levou a alguns insights:
* As pessoas que mais compram são solteiras, com curso superior e com nenhum filho.
* Somente 16% das campanhas de marketing resultaram em vendas. Necessário para ver o que melhorar nas próximas.
* EUA, Espanha e Chile foram os países com clientes que mais compraram.

## Capítulo 4 - Análise de dados Comerciais

Acesse ao dash pelo [link](https://app.powerbi.com/view?r=eyJrIjoiYTE4NGVjZTAtMDhlNi00OWY5LTg3MTMtMjE0OWUyZGU4M2QwIiwidCI6Ijg4YjY0MTgxLWQ5NGQtNGU4ZC1hNjNlLTIyZDg3ZWU5NDFkYyJ9).

Além de ser apresentado a alguns KPIs importantes da área comercial, foram vistos alguns novos gráficos como a Narrativa Inteligente, Principais influenciadores, 
o Gráfico de Faixas. Foram vistos alguns mais detalhes no Power Query como substituição de valores, tratamento básico de outliers.
Além de aprender como gerar um índice para as páginas do relatório.

A construção desse relatório levou a alguns insights:
* Algumas lojas são especializadas em algumas Categorias de produtos vendendo bastante em eletrodomésticos, porém sendo bem tímida em portáteis, por exemplo.
* O segmento doméstico domina em número de vendas e em valor, entretanto o segmento corporativo fez as vendas mais significativas.

## Capítulo 5 - Análise de dados de RH

Acesse ao dash pelo [link](https://app.powerbi.com/view?r=eyJrIjoiYzI5Y2RhY2ItZTQwZC00ODM4LTlmNTItOWIxNGY0OTMyN2M3IiwidCI6Ijg4YjY0MTgxLWQ5NGQtNGU4ZC1hNjNlLTIyZDg3ZWU5NDFkYyJ9).

Apresentação de importantes KPIs de RH.
Houve também a apresentação de mais expressões básicas em DAX: COUNT, COUNTDISTINCT, COUNTROWS, DIVIDE, AVERAGE, SUM, CALCULATE em uma tabela própria de medidas a fim de organizar e trazer rapidez nos cálculos do relatório.
Criação de colunas condicionais.
Foi dada uma pequena customização quanto a formatação de cores e formas do relatório.

As perguntas que teriam de ser respondidas foram:
1. Qual o total de funcionários atualmente na empresa?
2. Qual otempo médio de experiência dos funcionários (em anos)?
3. Qual o total e percentual de funcionários do gênero masculino e feminino?
4. Qual a média salarial mensal?
5. Qual o total de funcionários por função?
6. Qual o percentual defuncionários disponíveis para fazer hora extra?
7. Qual onível de envolvimento dos funcionários no trabalhoconsiderando 4 categorias: Ruim, Baixo, Médio e Alto?

