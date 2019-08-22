# Clusters de sapatos

## Introdução

Um dos maiores problemas do e-commerce é agrupar produtos baseado na visão do usuário e não em características como cor, preço e fabricante. Uma necessidade é corrigir preços.

Uma das formas de atacar este problema é usando as features conhecidas (preço, cores, fabricante, categorias da estrutura mercadológica, etc) par agerar um vetor que represente o produto, e encontrar clusters que os agrupem usando alguma métrica de distância vetorial.

Vamos atacar este problema com um dataset de sapatos.

## Dados

Temos um conjunto de dados de registros de vendas de sapatos, cada sapato está identificado por um *id* único, e as linhas o apresentam em diversas ofertas de lojas onlne com cores, a categorização do site em que é ofertado, condição da oferta (novo ou usado), marca e preço cobrado.


```python
import pandas as pd
df = pd.read_json("shoes.json", orient="records", lines=True)
df.head(10)
```

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>brand</th>
      <th>categories</th>
      <th>colors</th>
      <th>condition</th>
      <th>isSale</th>
      <th>price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AVpfEf_hLJeJML431ueH</td>
      <td>Naturalizer</td>
      <td>[Clothing, Heels, All Women's Shoes, Shoes, Sa...</td>
      <td>[Silver, Cream]</td>
      <td>USED</td>
      <td>False</td>
      <td>55.990</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AVpi74XfLJeJML43qZAc</td>
      <td>MUK LUKS</td>
      <td>[Clothing, All Women's Shoes, Women's Casual S...</td>
      <td>[Grey]</td>
      <td>NEW</td>
      <td>True</td>
      <td>41.125</td>
    </tr>
    <tr>
      <th>2</th>
      <td>AVpi74XfLJeJML43qZAc</td>
      <td>MUK LUKS</td>
      <td>[Clothing, All Women's Shoes, Women's Casual S...</td>
      <td>[Grey]</td>
      <td>NEW</td>
      <td>False</td>
      <td>35.250</td>
    </tr>
    <tr>
      <th>3</th>
      <td>AVpjXyCc1cnluZ0-V-Gj</td>
      <td>MUK LUKS</td>
      <td>[Clothing, All Women's Shoes, Shoes, Women's S...</td>
      <td>[Black]</td>
      <td>NEW</td>
      <td>False</td>
      <td>24.750</td>
    </tr>
    <tr>
      <th>4</th>
      <td>AVphGKLPilAPnD_x1Nrm</td>
      <td>MUK LUKS</td>
      <td>[Clothing, All Women's Shoes, Shoes, Women's S...</td>
      <td>[Grey]</td>
      <td>NEW</td>
      <td>True</td>
      <td>31.695</td>
    </tr>
    <tr>
      <th>5</th>
      <td>AVpg91ziilAPnD_xziOo</td>
      <td>Soft Ones</td>
      <td>[Clothing, All Womens Shoes, All Women's Shoes...</td>
      <td>[Brown]</td>
      <td>NEW</td>
      <td>True</td>
      <td>10.950</td>
    </tr>
    <tr>
      <th>6</th>
      <td>AVpjGKXyLJeJML43r8BH</td>
      <td>MUK LUKS</td>
      <td>[Clothing, Women's Casual Shoes, All Women's S...</td>
      <td>[Navy, Burgundy, Brown, Purple, Black, Natural...</td>
      <td>USED</td>
      <td>False</td>
      <td>18.395</td>
    </tr>
    <tr>
      <th>7</th>
      <td>AVpjGKXyLJeJML43r8BH</td>
      <td>MUK LUKS</td>
      <td>[Clothing, Women's Casual Shoes, All Women's S...</td>
      <td>[Navy, Burgundy, Brown, Purple, Black, Natural...</td>
      <td>USED</td>
      <td>False</td>
      <td>18.395</td>
    </tr>
    <tr>
      <th>8</th>
      <td>AVpfLXyhilAPnD_xWmNc</td>
      <td>MUK LUKS</td>
      <td>[Clothing, Shoes, Women's Shoes, All Women's S...</td>
      <td>[Grey, Navy]</td>
      <td>NEW</td>
      <td>True</td>
      <td>49.440</td>
    </tr>
    <tr>
      <th>9</th>
      <td>AVpfeWdJ1cnluZ0-lXYU</td>
      <td>MUK LUKS</td>
      <td>[Clothing, All Women's Shoes, Shoes, Women's B...</td>
      <td>[Brown]</td>
      <td>NEW</td>
      <td>True</td>
      <td>53.495</td>
    </tr>
  </tbody>
</table>
</div>



As colunas seguintes estão presentes:

* id: Código identificador de cada sapato
* brand: Marca do sapato
* categories: Categorias em que o sapato foi alocado no e-commerce onde a oferta é apresentada
* colors: Cores disponíveis
* condition: Se está usado ou novo.
* price: Preço cobrado pelo e-commerce em que está sendo ofertado.

## Desafio

1. Você deve usar o algoritmo de sua preferência para agrupar os sapatos de acordo com *condition*, *categories* e *colors* disponíveis gerando 4 clusters.

2. Para cada cluster encontrado, você deve calcular o preço médio e o desvio padrão de preço.

3. Como bônus, você pode apresentar os clusters num gráfico, mas essa etapa não é obrigatória.

Para tanto, tenha em mente as seguintes observações:

1. Um produto pode ser ofertado em mais de uma loja, portanto, pode aparecer mais de uma vez no conjunto de dados e ter valores para *categories*, *colors* e *condition* diferentes, você precisa reunir estes dados e criar um único registro para cada id.

2. Campos como *brand*, *categories*, *colors* e *condition* são **categóricos**, dependêndo do algoritmo utilizado você vai precisar aplicar alguma técnica como [codificação one-hot](https://medium.com/@arthurlambletvaz/one-hot-encoding-o-que-%C3%A9-cd2e8d302ae0) quando (e se) for utilizá-los.

3. Use a linguagem e a tecnologia que você domina. O código fonte da aplicação deve ser entregue junto com a associação entre *id* de sapato e o *id* de cluster correspondente e uma explicação de como funciona o algoritmo escolhido e por que o escolheu.

4. Você deve ser capaz de explicar o algoritmo escolhido, e todas as decisões que tomou numa entrevista técnica.

5. Escreva um código limpo e auto explicativo, mas se for necessário, você pode usar comentários de documentação.