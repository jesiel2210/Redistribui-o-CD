
# Nova Calculo de Redistribuição + Ponto de Pedido

## O que é?
O ato de dividir as mercadorias adquiridas entre as unidades de maneira prever a demanda e minimizar as transferências entre as unidades.

## Por que realizar?
Para evitar o retrabalho de guardar toda a mercadoria na localidade onde está recebendo a aquisição e depois ter o retrabalho de separar os itens, desta forma reduz consideravelmente o trabalho da logística.

## Definições


### Filiais.
Para o cálculo da necessidade deve ser considerada a quantidade mínima e a quantidade maxima, e  o que for maior, se algum for nulo o valor nulo deve ser desconsiderado.
Necessidade = estoque_virtual - (se quantidade_critica>quantidade_minima, quantidade_critica, quantidade_minima)


### CD.
Para o cálculo da necessidade deve ser considerada o quantidade critico. Definição da quantidade critico= (Demanda diária média x Lead Time) + Estoque de segurança. 

### Entradas.

- Prioridade de abastecimento da unidade (loja), na tabela lojas, coluna PRIORIDADE_ABASTECIMENTO, quanto menor o número, maior a prioridade. Hoje a ordem de prioridades no sistema:

LOJA | PRIORIDADE_ABASTECIMENTO
---: | ---:
1 | 100
2 | 11
3 | 3
4 | 200
5 | 2
6 | 4
7 | 8
8 | 7
9 | 5
10 | 9
11 | 10
12 | 1
13 | 6

- Dados do estoque do item na loja, na tabela produto_estoque, colunas QUANTIDADE_MINIMA, QUANTIDADE_CRITICO, ESTOQUE_VIRTUAL, EMPENHADO_ENCOMENDA e GIRO.
- Informações logística da movimentação do item, multiplo de movimentação interna. Tabela produtos_dbf, colunas EMBALAGEM_QUANTIDADE_EMPRESA e MOVIMENTA_FRACAO.
- O item da compra com a quantidade que foi entregue pelo fornecedor, tabela compras_itens_dbf coluna QUANTIDADE_ESTOQUE.

### Saidas

- Romaneios de transferência.

***
- compras_itens_redistribuicao
- compras_romaneios_redistribuicao
***

## Casos de exemplo (testes)

### Teste 01 - Item sem encomenda, com o multiplo de 1 item, entrando 12 unidades, para doze lojas com necessidade de 1 item em cada loja.

Entrada
produto_estoque
LOJA | QUANTIDADE_MINIMA | QUANTIDADE_MAXIMA | QUANTIDADE_CRITICA | ESTOQUE_VIRTUAL | EMPENHADO_ENCOMENDA | GIRO
---: | ---: | ---: | ---: | ---: | ---:| ---:
1 | 0 | 0 | 0 | 0 | 0
2 | 1 | 0 | 0 | 0 | 0
3 | 1 | 0 | 0 | 0 | 0
4 | 1 | 0 | 0 | 0 | 0
5 | 1 | 0 | 0 | 0 | 0
6 | 1 | 0 | 0 | 0 | 0
7 | 1 | 0 | 0 | 0 | 0
8 | 1 | 0 | 0 | 0 | 0
9 | 1 | 0 | 0 | 0 | 0
10| 1 | 0 | 0 | 0 | 0
11| 1 | 0 | 0 | 0 | 0
12| 1 | 0 | 0 | 0 | 0
13| 1 | 0 | 0 | 0 | 0

Resultado esperado, cada loja deverá ter um romaneio de transferência, com 1 unidade em cada.

Saidas
romaneio_item_dbf
LOJA | QUANTIDADE
---: | ---:
1 | 0
2 | 1
3 | 1
4 | 1
5 | 1
6 | 1
7 | 1
8 | 1
9 | 1
10| 1
11| 1
12| 1
13| 1



### Teste 02 - Item sem encomendas, com o multiplo de 1 item, entrando 8 unidades, para doze lojas com necessidade de 1 item em cada loja.

Entrada
produto_estoque
LOJA | QUANTIDADE_MINIMA | QUANTIDADE_MAXIMA | QUANTIDADE_CRITICA | ESTOQUE_VIRTUAL | EMPENHADO_ENCOMENDA | GIRO
---: | ---: | ---: | ---: | ---: | ---:| ---:
1 | 0 | 0 | 0 | 0 | 0
2 | 1 | 0 | 0 | 0 | 0
3 | 1 | 0 | 0 | 0 | 0
4 | 1 | 0 | 0 | 0 | 0
5 | 1 | 0 | 0 | 0 | 0
6 | 1 | 0 | 0 | 0 | 0
7 | 1 | 0 | 0 | 0 | 0
8 | 1 | 0 | 0 | 0 | 0
9 | 1 | 0 | 0 | 0 | 0
10| 1 | 0 | 0 | 0 | 0
11| 1 | 0 | 0 | 0 | 0
12| 1 | 0 | 0 | 0 | 0
13| 1 | 0 | 0 | 0 | 0



Lojas
LOJA | PRIORIDADE_ABASTECIMENTO
---: | ---:
1 | 100
2 | 11
3 | 3
4 | 200
5 | 2
6 | 4
7 | 8
8 | 7
9 | 5
10 | 9
11 | 10
12 | 1
13 | 6

Calculando o atendimento mínimo/crítico.

Divide-se as unidades pelo EMBALAGEM_QUANTIDADE_EMPRESA, considerando neste caso o valor 1.
Distribuindo = compra_item.QUANTIDADE_ESTOQUE.

Ordem de abastecimento das unidades pela prioridade = [12,5,3,6,9,13,8,7]

Ordenar as lojas por prioridade
Enquanto Distribuindo>0

Distribui um unidade para a loja da vez
Diminui a quantidade distribuindo.

 Resultado esperado, oito lojas deverá ter um romaneio de transferência, com 1 unidades em cada de acordo com a Prioridade de abastecimento da unidade (loja)

Saidas
romaneio_item_dbf
LOJA | QUANTIDADE
---: | ---:
1 | 0
2 | 0
3 | 1
4 | 0
5 | 1
6 | 1
7 | 1
8 | 1
9 | 1
10| 0
11| 0
12| 1
13| 1

### Teste 03 - Item sem encomendas, com o multiplo de 1 item, entrando 1200 unidades, para três lojas com necessidade de 48 itens em cada loja.

Entrada
produto_estoque
LOJA | QUANTIDADE_MINIMA | QUANTIDADE_MAXIMA | QUANTIDADE_CRITICA | ESTOQUE_VIRTUAL | EMPENHADO_ENCOMENDA | GIRO
---: | ---: | ---: | ---: | ---: | ---:| ---:
1 | 0 | 0 | 0 | 0 | 0
2 | 48| 0 | 0 | 0 | 0
3 | 48| 0 | 0 | 0 | 0
4 | 48| 0| 0 | 0 | 0
5 | 48| 0| 0 | 0 | 0
6 | 48| 0 | 0 | 0 | 0
7 | 48| 0 | 0 | 0 | 0
8 | 48| 0 | 0 | 0 | 0
9 | 48| 0 | 0 | 0 | 0
10| 48| 0 | 0 | 0 | 0
11| 48| 0 | 0 | 0 | 0
12| 48| 0| 0 | 0 | 0
13| 48| 0 | 0 | 0 |0

Resultado esperado, cada loja deverá ter um romaneio de transferência, com 48 unidades cada e o excedente 624 irá para o CD.

Saidas
romaneio_item_dbf
LOJA | QUANTIDADE
---: | ---:
1 | 624
2 | 48
3 | 48
4 | 48
5 | 48
6 | 48
7 | 48
8 | 48
9 | 48
10| 48
11| 48
12| 48
13| 48


### Teste 04 - Item sem encomendas, com o multiplo de 1 item, entrando 1200 unidades, para doze lojas com necessidade de 48 itens em cada loja e com QUANTIDADE MAXIMA de 96 em doze lojas.

Entrada
produto_estoque
LOJA | QUANTIDADE_MINIMA | QUANTIDADE_MAXIMA | QUANTIDADE_CRITICA | ESTOQUE_VIRTUAL | EMPENHADO_ENCOMENDA | GIRO
---: | ---: | ---: | ---: | ---: | ---:| ---:
1 | 0 | 0 | 0 | 0 | 0
2 | 48| 96 | 0 | 0 | 0
3 | 48| 96 | 0 | 0 | 0
4 | 48| 96| 0 | 0 | 0
5 | 48| 96| 0 | 0 | 0
6 | 48| 96 | 0 | 0 | 0
7 | 48| 96 | 0 | 0 | 0
8 | 48| 96 | 0 | 0 | 0
9 | 48| 96 | 0 | 0 | 0
10| 48| 96 | 0 | 0 | 0
11| 48| 96 | 0 | 0 | 0
12| 48| 96| 0 | 0 | 0
13| 48| 96 | 0 | 0 |0

Resultado esperado, cada loja deverá ter um romaneio de transferência, com 96 unidades cada e o excedente 48 irá para o CD.

Saidas
romaneio_item_dbf
LOJA | QUANTIDADE
---: | ---:
1 | 48
2 | 96
3 | 96
4 | 96
5 | 96
6 | 96
7 | 96
8 | 96
9 | 96
10| 96
11| 96
12| 96
13| 96




### Teste 05 - Item sem encomendas, com o multiplo de 1 item, entrando 200 unidades, para doze lojas com necessidade  de 48 itens em cada loja e com QUANTIDADE MAXIMA de 96 em quatro lojas.

Entrada
produto_estoque
LOJA | QUANTIDADE_MINIMA | QUANTIDADE_MAXIMA | QUANTIDADE_CRITICA | ESTOQUE_VIRTUAL | EMPENHADO_ENCOMENDA | GIRO
---: | ---: | ---: | ---: | ---: | ---:| ---:
1 | 0 | 0 | 0 | 0 | 0
2 | 48|0 | 0 | 0 | 0
3 | 48|0 | 0 | 0 | 0
4 | 48|0 | 0 | 0 | 0
5 | 48| 96| 0 | 0 | 0
6 | 48| 96 | 0 | 0 | 0
7 | 48| 0 | 0 | 0 | 0
8 | 48| 0 | 0 | 0 | 0
9 | 48| 96 | 0 | 0 | 0
10| 48| 0 | 0 | 0 | 0
11| 48| 0 | 0 | 0 | 0
12| 48| 96| 0 | 0 | 0
13| 48| 0 | 0 | 0 |0



Lojas
LOJA | PRIORIDADE_ABASTECIMENTO
---: | ---:
1 | 100
2 | 11
3 | 3
4 | 200
5 | 2
6 | 4
7 | 8
8 | 7
9 | 5
10 | 9
11 | 10
12 | 1
13 | 6

Calculando o atendimento mínimo/crítico.

Divide-se as unidades pelo EMBALAGEM_QUANTIDADE_EMPRESA, considerando neste caso o valor 1.
Distribuindo = compra_item.QUANTIDADE_ESTOQUE.

Ordem de abastecimento das unidades pela prioridade = [12,5,3,6,9]

Ordenar as lojas por prioridade
Enquanto Distribuindo>0
Distribui um unidade para a loja da vez
Diminui a quantidade distribuindo.

Resultado esperado, quatro lojas deverá ter um romaneio de transferência, com 48 unidades em cada de acordo com a Prioridade de abastecimento da unidade (loja), e uma loja ira receber 02 itens de acordo com a prioridade_abastecimento

Saidas
romaneio_item_dbf
LOJA | QUANTIDADE
---: | ---:
1 | 0
2 | 0
3 | 48
4 | 0
5 | 48
6 | 48
7 | 0
8 | 0
9 | 2
10| 0
11| 0
12| 48
13| 0



### Teste 6 - Item sem encomendas, com o multiplo de 1 item, entrando 600 unidades, para doze lojas com necessidade 16 itens em cada loja e com QUANTIDADE MAXIMA de 32 em quatro lojas.

Entrada
produto_estoque
LOJA | QUANTIDADE_MINIMA | QUANTIDADE_MAXIMA | QUANTIDADE_CRITICA | ESTOQUE_VIRTUAL | EMPENHADO_ENCOMENDA | GIRO
---: | ---: | ---: | ---: | ---: | ---:| ---:
1 | 0 | 0 | 0 | 0 | 0
2 | 16|0 | 0 | 0 | 0
3 | 16|0 | 0 | 0 | 0
4 | 16|0 | 0 | 0 | 0
5 | 16| 32| 0 | 0 | 0
6 | 16| 32 | 0 | 0 | 0
7 | 16| 0 | 0 | 0 | 0
8 | 16| 0 | 0 | 0 | 0
9 | 16| 32 | 0 | 0 | 0
10| 16| 0 | 0 | 0 | 0
11| 16| 0 | 0 | 0 | 0
12| 16| 32| 0 | 0 | 0
13| 16| 0 | 0 | 0 |0


Resultado esperado, oito lojas deverá ter um romaneio de transferência  com 16 unidades cada, quatros lojas irá receber 32 unidades,  e o CD ficara com o excesso 344 unidades.

Saidas
romaneio_item_dbf
LOJA | QUANTIDADE
---: | ---:
1 | 344
2 | 16
3 | 16
4 | 16
5 | 32
6 | 32
7 | 16
8 | 16
9 | 32
10| 16
11| 16
12| 32
13| 16

### Teste 07 - Item sem encomendas, com o multiplo de 4 item, entrando  600 unidades, para doze lojas com necessidade de 16 itens em cada loja.

Entrada
produto_estoque
LOJA | QUANTIDADE_MINIMA | QUANTIDADE_MAXIMA | QUANTIDADE_CRITICA | ESTOQUE_VIRTUAL | EMPENHADO_ENCOMENDA | GIRO
---: | ---: | ---: | ---: | ---: | ---:| ---:
1 | 0 | 0 | 0 | 0 | 0
2 | 16|0 | 0 | 0 | 0
3 | 16|0 | 0 | 0 | 0
4 | 16|0 | 0 | 0 | 0
5 | 16| 0| 0 | 0 | 0
6 | 16| 0 | 0 | 0 | 0
7 | 16| 0 | 0 | 0 | 0
8 | 16| 0 | 0 | 0 | 0
9 | 16| 0 | 0 | 0 | 0
10| 16| 0 | 0 | 0 | 0
11| 16| 0 | 0 | 0 | 0
12| 16| 0| 0 | 0 | 0
13| 16| 0 | 0 | 0 |0

Resultado esperado, doze lojas deverá ter um romaneio de transferência  com 16 unidades cada, e o CD ficara com o excesso 192 unidades.

Saidas
romaneio_item_dbf
LOJA | QUANTIDADE
---: | ---:
1 | 192
2 | 16
3 | 16
4 | 16
5 | 16
6 | 16
7 | 16
8 | 16
9 | 16
10| 16
11| 16
12| 16
13| 16

### Teste 08 - Item com uma encomenda, com o multiplo de 1 item, entrando 60 unidades, para doze lojas com necessidade de 16 item em cada loja.


Entrada
produto_estoque
LOJA | QUANTIDADE_MINIMA | QUANTIDADE_MAXIMA | QUANTIDADE_CRITICA | ESTOQUE_VIRTUAL | EMPENHADO_ENCOMENDA | GIRO
---: | ---: | ---: | ---: | ---: | ---:| ---:
1 | 0 | 0 | 0 | 0 | 0
2 | 16|0 | 0 | 0 | 0
3 | 16|0 | 0 | 0 | 0
4 | 16|0 | 0 | 0 | 0
5 | 16| 0| 0 | 0 | 0
6 | 16| 0 | 0 | 0 | 0
7 | 16| 0 | 0 | 0 | 0
8 | 16| 0 | 0 | 1 | 0
9 | 16| 0 | 0 | 0 | 0
10| 16| 0 | 0 | 0 | 0
11| 16| 0 | 0 | 0 | 0
12| 16| 0| 0 | 0 | 0
13| 16| 0 | 0 | 0 |0


Lojas
LOJA | PRIORIDADE_ABASTECIMENTO
---: | ---:
1 | 100
2 | 11
3 | 3
4 | 200
5 | 2
6 | 4
7 | 8
8 | 7
9 | 5
10 | 9
11 | 10
12 | 1
13 | 6


Calculando o atendimento mínimo/crítico.

Divide-se as unidades pelo EMBALAGEM_QUANTIDADE_EMPRESA, considerando neste caso o valor 1.
Distribuindo = compra_item.QUANTIDADE_ESTOQUE.

Ordem de abastecimento das unidades pela prioridade se altera levando em consideração em qual lojas tem empenhado_encomenda  = [8,12,5,3,6]

Ordenar as lojas por prioridade
Enquanto Distribuindo>0
A loja que tem empenhado_encomenda tem a primeira prioridade,  seguindo para a proxima loja da vez seguindo o criterio de prioridade.
 diminui a quantidade distribuindo.
 
Resultado esperado, três lojas deverá ter um romaneio de transferência  com 16 unidades cada,uma loja irá receber um romaneio com 11 unidades,sendo a que tem a loja que tem empenhado_econcomenda recebe 1 romaneio com um item.


Saidas
romaneio_item_dbf
LOJA | QUANTIDADE
---: | ---:
1 | 0
2 | 0
3 | 16
4 | 0
5 | 16
6 | 11
7 | 0
8 | 1
9 | 0
10| 0
11| 0
12| 16
13| 0

### Teste 09 - Item sem encomendas, com o multiplo de 4 item, entrando 60 unidades, para doze lojas com necessidade de 16 item em cada loja.


Entrada
produto_estoque
LOJA | QUANTIDADE_MINIMA | QUANTIDADE_MAXIMA | QUANTIDADE_CRITICA | ESTOQUE_VIRTUAL | EMPENHADO_ENCOMENDA | GIRO
---: | ---: | ---: | ---: | ---: | ---:| ---:
1 | 0 | 0 | 0 | 0 | 0
2 | 16|0 | 0 | 0 | 0
3 | 16|0 | 0 | 0 | 0
4 | 16|0 | 0 | 0 | 0
5 | 16| 0| 0 | 0 | 0
6 | 16| 0 | 0 | 0 | 0
7 | 16| 0 | 0 | 0 | 0
8 | 16| 0 | 0 | 0 | 0
9 | 16| 0 | 0 | 0 | 0
10| 16| 0 | 0 | 0 | 0
11| 16| 0 | 0 | 0 | 0
12| 16| 0| 0 | 0 | 0
13| 16| 0 | 0 | 0 |0


Lojas
LOJA | PRIORIDADE_ABASTECIMENTO
---: | ---:
1 | 100
2 | 11
3 | 3
4 | 200
5 | 2
6 | 4
7 | 8
8 | 7
9 | 5
10 | 9
11 | 10
12 | 1
13 | 6


Calculando o atendimento mínimo/crítico.

Divide-se as unidades pelo EMBALAGEM_QUANTIDADE_EMPRESA, considerando neste caso o valor 1.

Distribuindo = compra_item.QUANTIDADE_ESTOQUE.

Ordem de abastecimento das unidades pela prioridade = [12,5,3,6]

Ordenar as lojas por prioridade
Enquanto Distribuindo>0

Distribui um unidade para a loja da vez

Diminui a quantidade distribuindo.

Resultado esperado, três lojas deverá ter um romaneio de transferência  com 16 unidades cada, e um loja irá receber 12 unidades de acordo com a prioridade_abastecimento


Saidas
romaneio_item_dbf
LOJA | QUANTIDADE
---: | ---:
1 | 0
2 | 0
3 | 16
4 | 0
5 | 16
6 | 12
7 | 0
8 | 0
9 | 0
10| 0
11| 0
12| 16
13| 0

### Teste 10 - Item sem encomendas, com o multiplo de 4 item, entrando 600 unidades, para doze lojas com necessidade 16 itens em cada loja e com QUANTIDADE MAXIMA de 32 em quatro lojas.

Entrada
produto_estoque
LOJA | QUANTIDADE_MINIMA | QUANTIDADE_MAXIMA | QUANTIDADE_CRITICA | ESTOQUE_VIRTUAL | EMPENHADO_ENCOMENDA | GIRO
---: | ---: | ---: | ---: | ---: | ---:| ---:
1 | 0 | 0 | 0 | 0 | 0
2 | 16|0 | 0 | 0 | 0
3 | 16|0 | 0 | 0 | 0
4 | 16|0 | 0 | 0 | 0
5 | 16| 32| 0 | 0 | 0
6 | 16| 32 | 0 | 0 | 0
7 | 16| 0 | 0 | 0 | 0
8 | 16| 0 | 0 | 0 | 0
9 | 16| 32 | 0 | 0 | 0
10| 16| 0 | 0 | 0 | 0
11| 16| 0 | 0 | 0 | 0
12| 16| 32| 0 | 0 | 0
13| 16| 0 | 0 | 0 |0


Resultado esperado, oito lojas deverá ter um romaneio de transferência  com 16 unidades cada, quatros lojas irá receber 32 unidades,  e o CD ficara com o excesso 344 unidades.

Saidas
romaneio_item_dbf
LOJA | QUANTIDADE
---: | ---:
1 | 344
2 | 16
3 | 16
4 | 16
5 | 32
6 | 32
7 | 16
8 | 16
9 | 32
10| 16
11| 16
12| 32
13| 16



### Teste 11 - Item sem encomendas, com o multiplo de 4 item, entrando 60 unidades, para doze lojas com necessidade de 16 item em cada loja.


Entrada
produto_estoque
LOJA | QUANTIDADE_MINIMA | QUANTIDADE_MAXIMA | QUANTIDADE_CRITICA | ESTOQUE_VIRTUAL | EMPENHADO_ENCOMENDA | GIRO
---: | ---: | ---: | ---: | ---: | ---:| ---:
1 | 0 | 0 | 0 | 0 | 0
2 | 16|0 | 0 | 0 | 0
3 | 16|0 | 0 | 0 | 0
4 | 16|0 | 0 | 0 | 0
5 | 16| 0| 0 | 0 | 0
6 | 16| 0 | 0 | 0 | 0
7 | 16| 0 | 0 | 0 | 0
8 | 16| 0 | 0 | 0 | 0
9 | 16| 0 | 0 | 0 | 0
10| 16| 0 | 0 | 0 | 0
11| 16| 0 | 0 | 0 | 0
12| 16| 0| 0 | 0 | 0
13| 16| 0 | 0 | 0 |0


Lojas
LOJA | PRIORIDADE_ABASTECIMENTO
---: | ---:
1 | 100
2 | 11
3 | 3
4 | 200
5 | 2
6 | 4
7 | 8
8 | 7
9 | 5
10 | 9
11 | 10
12 | 1
13 | 6


Calculando o atendimento mínimo/crítico.

Divide-se as unidades pelo EMBALAGEM_QUANTIDADE_EMPRESA, considerando neste caso o valor 4.
Distribuindo = compra_item.QUANTIDADE_ESTOQUE.

Ordem de abastecimento das unidades pela prioridade se altera levando em consideração em qual lojas tem empenhado_encomenda  = [12,5,3,6]

Ordenar as lojas por prioridade
Enquanto Distribuindo>0
Diminui a quantidade distribuindo.
 
Resultado esperado, três lojas deverá ter um romaneio de transferência  com 16 unidades cada,uma loja irá receber um romaneio com 12 unidades.

Saidas
romaneio_item_dbf
LOJA | QUANTIDADE
---: | ---:
1 | 0
2 | 0
3 | 16
4 | 0
5 | 16
6 | 12
7 | 0
8 | 0
9 | 0
10| 0
11| 0
12| 16
13| 0

### ### Teste 12 - Item com quatro encomendas, com o multiplo de 1 item, entrando 3 unidades, para doze lojas com necessidade de 1 item em cada loja.

produto_estoque
LOJA | QUANTIDADE_MINIMA | QUANTIDADE_CRITICO | ESTOQUE_VIRTUAL | EMPENHADO_ENCOMENDA | GIRO
---: | ---: | ---: | ---: | ---: | ---:
1 | 0 | 0 | 0 | 0 | 0
2 | 1 | 0 | 0 | 1 | 0
3 | 1 | 0 | 0 | 1 | 0
4 | 1 | 0 | 0 | 0 | 0
5 | 1 | 0 | 0 | 1 | 0
6 | 1 | 0 | 0 | 0 | 0
7 | 1 | 0 | 0 | 0 | 0
8 | 1 | 0 | 0 | 0 | 0
9 | 1 | 0 | 0 | 0 | 0
10| 1 | 0 | 0 | 1 | 0
11| 1 | 0 | 0 | 0 | 0
12| 1 | 0| 0 | 0 | 0
13| 1| 0 | 0 | 0 |0


Lojas
LOJA | PRIORIDADE_ABASTECIMENTO
---: | ---:
1 | 100
2 | 11
3 | 3
4 | 200
5 | 2
6 | 4
7 | 8
8 | 7
9 | 5
10 | 9
11 | 10
12 | 1
13 | 6

Loja

Calulando o atendimento mínimo/crítico.

Divide-se as unidades pelo EMBALAGEM_QUANTIDADE_EMPRESA, considerando neste caso o valor 1.

Distribuindo = compra_item.QUANTIDADE_ESTOQUE.

Ordenar as lojas por prioridade

Enquanto Distribuindo>0

A lojas que tem empenhado_encomenda tem a primeira prioridade,  seguindo para a proxima loja da vez seguindo o criterio de prioridade, e a data e hora do empenhado_encomenda.

Diminui a quantidade distribuindo.


Loja 2- empenhado_encomenda 10/10/2022 as 9:00- recebe 01

Loja 3- empenhado_encomenda 10/10/2022 as 10:00- recebe 01 


Loja 5- empenhado_encomenda 10/10/2022 as 8:30 - recebe 01 

Loja 10- empenhado_encomenda 10/10/2022 as 10:30- recebe na proxima entrada como prioridade


Saidas
romaneio_item_dbf
LOJA | Quantidade
---: | ---:
1 | 0
2 | 1
3 | 1
4 | 0
5 | 1
6 | 0
7 | 0
8 | 0
9 | 0
10 | 0
11 | 0
12 | 0
13 | 0

### ### Teste 13 -  Item sem encomendas, com múltiplos de 2 itens, entrando 40 unidades, para doze lojas com quantidades minima diferente em cada loja

Entrada
produto_estoque
LOJA | QUANTIDADE_MINIMA | QUANTIDADE_CRITICO | ESTOQUE_VIRTUAL | EMPENHADO_ENCOMENDA | GIRO
---: | ---: | ---: | ---: | ---: | ---:
1|	0|	0 | 0 | 0 | 0
2|10| 0 | 0 | 0 | 0
3|15| 0 | 0 | 0 | 0
4|8|	 0 | 0 | 0 | 0
5|12|	0 | 0 | 0 | 0
6|20|	0 | 0 | 0 | 0
7|14|0 | 0 | 0 | 0
8|11|0 | 0 | 0 | 0
9|16|	0 | 0 | 0 | 0
10|9|	0 | 0 | 0 | 0
11|13|	0 | 0 | 0 | 0
12|18|	0 | 0 | 0 | 0
13|21|	0 | 0 | 0 | 0


Lojas
LOJA | PRIORIDADE_ABASTECIMENTO
---: | ---:
1 | 100
2 | 11
3 | 3
4 | 200
5 | 2
6 | 4
7 | 8
8 | 7
9 | 5
10 | 9
11 | 10
12 | 1
13 | 6

Calculando o atendimento mínimo/crítico.

Divide-se as unidades pelo EMBALAGEM_QUANTIDADE_EMPRESA, considerando neste caso o valor 2.
Distribuindo = compra_item.QUANTIDADE_ESTOQUE.

Ordem de abastecimento das unidades pela prioridade   = [12, 5, 6, ]
Ordenar as lojas por prioridade
Enquanto Distribuindo>0
Diminui a quantidade distribuindo.
 
Resultado esperado, loja 12 deverá ter um romaneio de transferência  com 18 unidades,loja 5 irá receber um romaneio com 12 unidades, loja 3 recebera um romaneio com 16 unidades e loja 06 um romaneio com 04 unidades 




# Ponto de Pedido
## O que é?
O ponto de pedido é o nível de estoque em que um novo pedido deve ser feito para evitar a ruptura de estoque. É calculado com base na demanda diária média, no tempo de reposição (lead time) e no estoque de segurança. Ele garante que você tenha produtos suficientes para atender à demanda sem excessos.

Itens Curva A,B por X 

| Código  | Descrição | Giro  | Giro Curva | Venda Diária (uni) | Venda por Semana (uni) | Venda Mensal | Lead Time |
|---------|-----------|-------|------------|--------------------|------------------------|--------------|-----------|
| 17766   | OLEO MOTOR 1 LITRO SAE 15W40 API SL SEMI SINTETICO | 13952 | A | 89 | 537 | 2146 | 4 |
| 17655   | OLEO MOTOR 1 LITRO SL SAE 20W50 MINERAL RADNAQ | 12826 | A | 82 | 493 | 1973 | 4 |
| 10376   | FILTRO OLEO MONZA/KADETT 82/ OMEGA/VECTRA/CORSA/MONTANA/ASTRA/S10/ZAFIRA 1.8/2.0/2.2/2.4 CORSA/CELTA/MONTANA 1.0/1.4/1.6 8V 95/ - SPIN/COBALT 1.4/1.8 8V 2012/ AGILE 09/ PALIO/WEEKEND/SIENA/STRADA/DOBLO/IDEA/PUNTO 1.8 03/ - NOVA S 10 2.4 2012/ - ONIX/PRISMA 1.0/1.4 8V 2012/ | 1885 | B | 12 | 73 | 290 | 15 |
| 29057   | VELA GOL/PARATI 1.0 8V 05/ - FOX/POLO 1.0 8V 05/  TOTAL FLEX E GNV - FOX/GOL/VOYAGE/SAVEIRO FLEX 1.0/1.6 8V 08/ - KOMBI 1.4 FLEX 09/ 1 ELETRODO | 3468 | B | 22 | 133 | 534 | 3 |
| 29253   | VELA RESISTIVA CELTA/CORSA 1.0 8V 05/ FLEXPOWER ONIX/PRISMA 1.0 8V 2013/ | 1762 | B | 11 | 68 | 271 | 3 |
| 27650   | VELA RESISTIVA GREEN GASOL/ALC. FIAT/PEUGEOT/RENAULT/CITROEN 1 ELETRODO | 2989 | B | 19 | 115 | 460 | 3 |
| 23980   | ADITIVO AGUA RADIADOR ORGANICO ROSA 1L BIO SOLUCAO ORGANICO LONG LIFE DILUIDO (2 ANOS SISTEMA) | 1619 | B | 10 | 62 | 249 | 4 |
| 23950   | ADITIVO AGUA RADIADOR ORGANICO ROSA 1L BIOFLUIDO LONG LIFE PARAFLU (SEMI SINTETICO 2 ANOS SISTEMA) | 4995 | B | 32 | 192 | 768 | 4 |
| 969     | FLUIDO FREIO 500ML DOT 3 | 1852 | B | 12 | 71 | 285 | 4 |
| 4969    | FLUIDO FREIO 500ML DOT 4 - E FLUIDO P/ SISTEMAS EMBREAGEM | 2465 | B | 16 | 95 | 379 | 4 |
| 13816   | OLEO HIDRAULICO 1 LITRO ATF SUFIXO A | 2364 | B | 15 | 91 | 364 | 4 |
| 17760   | OLEO MOTOR 1 LITRO SAE 10W40 API SN SEMI SINTETICO | 1757 | B | 11 | 68 | 270 | 4 |
| 29398   | OLEO MOTOR 1 LITRO SN SAE 5W30 SINTETICO GM S10 8V/COBALT 8V /ONIX 8V/SPIN 8V GASOLINA/FLEX/GNV 2012/ - FORD ECOSPORT/FIESTA/KA 1.0/1.6 ROCAM E SIGMA  (DEXOS 1 GM6094M / ILSAC GF 5 / FORD WSSM2C946B1 / CHRYSLER MS6395) | 6577 | B | 42 | 253 | 1012 | 4 |
| 107198  | OLEO MOTOR 1 LITRO SP SAE 5W30 SINTETICO GM S10/COBALT/ONIX/SPIN GASOLINA/FLEX/GNV 2012/ - FORD ECOSPORT/FIESTA/KA 1.0/1.6 ROCAM E SIGMA  (ILSAC GF 6 / FORD WSSM2C946B1 / CHRYSLER MS6395 ) | 1661 | B | 11 | 64 | 256 | 4 |
| 24800   | OLEO MOTOR 1LT. SINTETICO 5W40 API-SN (MB-APPROVAL 229.3 PORSCHE A40, RENAULT RN0700/RN0710, VW (GASOLINA/DIESEL), 502.00/505.00, PSA (PEUGEOT/CITROEN). | 3078 | B | 20 | 118 | 474 | 4 |
| 13822   | OLEO TRANSMISSAO 1 LITRO SAE 90 CAMBIO/DIFER API GL5 | 2169 | B | 14 | 83 | 334 | 4 |
| 25303   | AGUA DESMINERALIZADA E DEIONIZADA 1 LITRO PARA BATERIAS E SISTEMAS DE RADIADOR) | 1763 | B | 11 | 68 | 271 | 15 |
| 99904   | AGUA DESMINERALIZADA E DEIONIZADA 5 LITROS PARA BATERIAS E SISTEMAS DE RADIADOR) | 3566 | B | 23 | 137 | 549 | 15 |
| 5852    | ANTI-CORROSIVO SPRAY 300ML STARRETT LUB | 1647 | B | 11 | 63 | 253 | 10 |
| 20558   | SILICONE 85GR PRETO ULTRA BLACK SI598 (OXIMICO) | 2596 | B | 17 | 100 | 399 | 3 |
| 9700    | FAIXA REFLETIVA PARA CAMINHOES DIREITO | 4778 | B | 31 | 184 | 735 | 10 |



Calculo de Ponto de Pedido



| Código  | Descrição | Demanda Diária Média | Lead Time | Ponto de Pedido |
|---------|-----------|-----------------------|-----------|-----------------|
| 17766   | OLEO MOTOR 1 LITRO SAE 15W40 API SL SEMI SINTETICO | 89  | 4 | 356 |
| 17655   | OLEO MOTOR 1 LITRO SL SAE 20W50 MINERAL RADNAQ | 82  | 4 | 328 |
| 10376   | FILTRO OLEO MONZA/KADETT 82/ OMEGA/VECTRA/CORSA/MONTANA/ASTRA/S10/ZAFIRA 1.8/2.0/2.2/2.4 CORSA/CELTA/MONTANA 1.0/1.4/1.6 8V 95/ - SPIN/COBALT 1.4/1.8 8V 2012/ AGILE 09/ PALIO/WEEKEND/SIENA/STRADA/DOBLO/IDEA/PUNTO 1.8 03/ - NOVA S 10 2.4 2012/ - ONIX/PRISMA 1.0/1.4 8V 2012/ | 12  | 15 | 180 |
| 29057   | VELA GOL/PARATI 1.0 8V 05/ - FOX/POLO 1.0 8V 05/  TOTAL FLEX E GNV - FOX/GOL/VOYAGE/SAVEIRO FLEX 1.0/1.6 8V 08/ - KOMBI 1.4 FLEX 09/ 1 ELETRODO | 22  | 3 | 66 |
| 29253   | VELA RESISTIVA CELTA/CORSA 1.0 8V 05/ FLEXPOWER ONIX/PRISMA 1.0 8V 2013/ | 11  | 3 | 33 |
| 27650   | VELA RESISTIVA GREEN GASOL/ALC. FIAT/PEUGEOT/RENAULT/CITROEN 1 ELETRODO | 19  | 3 | 57 |
| 23980   | ADITIVO AGUA RADIADOR ORGANICO ROSA 1L BIO SOLUCAO ORGANICO LONG LIFE DILUIDO (2 ANOS SISTEMA) | 10  | 4 | 40 |
| 23950   | ADITIVO AGUA RADIADOR ORGANICO ROSA 1L BIOFLUIDO LONG LIFE PARAFLU (SEMI SINTETICO 2 ANOS SISTEMA) | 32  | 4 | 128 |
| 969     | FLUIDO FREIO 500ML DOT 3 | 12  | 4 | 48 |
| 4969    | FLUIDO FREIO 500ML DOT 4 - E FLUIDO P/ SISTEMAS EMBREAGEM | 16  | 4 | 64 |
| 13816   | OLEO HIDRAULICO 1 LITRO ATF SUFIXO A | 15  | 4 | 60 |
| 17760   | OLEO MOTOR 1 LITRO SAE 10W40 API SN SEMI SINTETICO | 11  | 4 | 44 |
| 29398   | OLEO MOTOR 1 LITRO SN SAE 5W30 SINTETICO GM S10 8V/COBALT 8V /ONIX 8V/SPIN 8V GASOLINA/FLEX/GNV 2012/ - FORD ECOSPORT/FIESTA/KA 1.0/1.6 ROCAM E SIGMA  (DEXOS 1 GM6094M / ILSAC GF 5 / FORD WSSM2C946B1 / CHRYSLER MS6395) | 42  | 4 | 168 |
| 107198  | OLEO MOTOR 1 LITRO SP SAE 5W30 SINTETICO GM S10/COBALT/ONIX/SPIN GASOLINA/FLEX/GNV 2012/ - FORD ECOSPORT/FIESTA/KA 1.0/1.6 ROCAM E SIGMA  (ILSAC GF 6 / FORD WSSM2C946B1 / CHRYSLER MS6395 ) | 11  | 4 | 44 |
| 24800   | OLEO MOTOR 1LT. SINTETICO 5W40 API-SN (MB-APPROVAL 229.3 PORSCHE A40, RENAULT RN0700/RN0710, VW (GASOLINA/DIESEL), 502.00/505.00, PSA (PEUGEOT/CITROEN). | 20  | 4 | 80 |
| 13822   | OLEO TRANSMISSAO 1 LITRO SAE 90 CAMBIO/DIFER API GL5 | 14  | 4 | 56 |
| 25303   | AGUA DESMINERALIZADA E DEIONIZADA 1 LITRO PARA BATERIAS E SISTEMAS DE RADIADOR) | 11  | 15 | 165 |
| 99904   | AGUA DESMINERALIZADA E DEIONIZADA 5 LITROS PARA BATERIAS E SISTEMAS DE RADIADOR) | 23  | 15 | 345 |
| 5852    | ANTI-CORROSIVO SPRAY 300ML STARRETT LUB | 11  | 10 | 110 |
| 20558   | SILICONE 85GR PRETO ULTRA BLACK SI598 (OXIMICO) | 17  | 3 | 51 |
| 9700    | FAIXA REFLETIVA PARA CAMINHOES DIREITO | 31  | 10 | 310 |


