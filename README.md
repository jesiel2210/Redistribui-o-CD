
# Nova Redistribuição

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



Resultado esperado, quatro lojas deverá ter um romaneio de transferência, com 48 unidades em cada de acordo com a Prioridade de abastecimento da unidade (loja), e uma loja ira receber 02 itens de acordo com a prioridade_abastecimento

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
 diminui a quantidade distribuindo.

Loja 2- empenhado_encomenda 10/10/2022 as 9:00- recebe 01

Loja 3- empenhado_encomenda 10/10/2022 as 10:00- recebe 01 


Loja 5- empenhado_encomenda 10/10/2022 as 8:30 - recebe 01 

Loja 10- empenhado_encomenda 10/10/2022 as 10:30- recebe na proxima entrada como prioridade


Saidas
romaneio_item_dbf
LOJA | PRIORIDADE_ABASTECIMENTO
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






