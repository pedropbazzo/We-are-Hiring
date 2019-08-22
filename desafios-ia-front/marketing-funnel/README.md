# Funil de conversão

## Descrição

Um dos problemas de otimização de conversão está na `análise de funil`. O processo de venda passa por várias etapas, e O funil de conversão diz respeito a quantidade de elementos que atravessam para a próxima etapa.

A Olist é um seller da B2W que auxilia pequenos sellers a vender em diversos marketplaces,eles oferecem um conjunto de dados públicos do seu funil de cadastro de sellers que fica disponível para estudo e pdoe ser encontrado na pasta *dataset*.

Neste caso, as etapas do funil são:

1. O lead inscreve-se em uma página de cadastro.
2. O lead é contatado por um Representante de Desenvolvimento de Vendas (SDR), algumas informações são confirmadas e uma consultoria é agendada.
3. A consultoria é feita por um representante de vendas (SR). O SR pode fechar o negócio (o lead se cadastra) ou não (o lead abandona sem concluir o cadastro)
4. O lead se torna um vendedor e começa a criar seu catálogo no Olist.
5. Os produtos são publicados em marketplaces e ficam prontos para venda.

Um ponto importante é que um seller pode vir de várias fontes, isso é, pode se cadastrar a partir de diversas origens.

## Dados 

Os dados possuem o seguinte schema:

![Schema de dados do funil de vendas da Olist](schema-olist.png)

As tabelas constituintes são:

### `olist_marketing_qualified_leads_dataset.csv`

Depois que um lead preenche um formulário em uma página de cadastro, um filtro é feito para selecionar aqueles que estão qualificados. Estes são os *Leads Qualificados de Marketing* (MQLs).

As colunas deste conjunto de dados são:

* mql_id: ID de lead qualificado
* first_contact_date: data da primeira solicitação de contato.
* landing_page_id: ID da página de cadastro em que o lead foi adquirido
* origem: tipo de mídia em que o lead foi adquirido

### `olist_closed_deals_dataset.csv`

Depois que um lead qualificado preenche um formulário em uma página de cadastro, ele é contatado por um Representante de Desenvolvimento de Vendas. Nesta etapa, algumas informações são verificadas e mais informações sobre o lead são coletadas.

* mql_id: ID de lead qualificado
* seller_id: código do seller
* sdr_id: ID do representante de desenvolvimento de vendas
* sr_id: representante de vendas
* won_date: Data em que o negócio foi fechado.
* business_segment: segmento de negócios que o Lead informou no contato.
* lead_type: Tipo de lead.
* lead_behaviour_profile: Perfil de comportamento do lead
* has_company: O lead tem uma empresa (documentação formal)?
* has_gtin: O lead tem o Número Global de Item Comercial (código de barras) para seus produtos?
* average_stock: estoque médio declarado.
* business_type: tipo de empresa (revendedor / fabricante etc.)
* declared_product_catalog_size: tamanho do catálogo declarado.
* declared_monthly_revenue: receita mensal estimada declarada pelo lead.

## Desafio

### Treinamento:

A ideia é encontrar uma forma de pontuar os leads de acordo com a estimativa de fechamento de contrato, isto é, criar estimativas de cada lead ter o negócio fechado ou não.

* Use o método e as bibliotecas que preferir para criar uma **aplicação** Spark (com a API Scala), que use cross validation e tuning de hiperparâmetros (se houver) para encontrar essas estimativas.
* Um bônus é encontrar quais as features de maior relevância para o resultado.
* O resultado dessa etapa é um modelo treinado, junto com as métricas de desempenho.

### Inferência

* Com o modelo gerado na etapa anterior, crie um sistema para fazer inferência em resultados lidos de um arquivo CSV e um arquivo JSON apresentando a estimativa para cada um deles.
* Um bônus é criar uma API para servir este modelo.