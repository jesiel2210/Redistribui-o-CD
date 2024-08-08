
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



### Teste 02 - Item sem encomendas, com o multiplo de 1 item, entrando 8 unidades, para doze lojas com necessidade QUANTIDADE_MINIMA de 1 item em cada loja.

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


Resultado esperado, oito lojas deverá ter um romaneio de transferência, com 1 unidades em cada de acordo com a Prioridade de abastecimento da unidade (loja), na tabela lojas, coluna PRIORIDADE_ABASTECIMENTO

romaneio_item_dbf
LOJA | QUANTIDADE
---: | ---:
1 | 0
2 | 0
3 | 1
4 | 1
5 | 1
6 | 1
7 | 0
8 | 1
9 | 0
10| 0
11| 1
12| 1
13| 1

### Teste 03 - Item sem encomendas, com o multiplo de 1 item, entrando 1200 unidades, para três lojas com necessidade QUANTIDADE MININA  de 48 item em cada loja.


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


### Teste 04 - Item sem encomendas, com o multiplo de 1 item, entrando 1200 unidades, para doze lojas com necessidade QUANTIDADE MININA  de 48 item em cada loja e com QUANTIDADE MAXIMA de 96 em doze lojas.


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




### Teste 05 - Item sem encomendas, com o multiplo de 1 item, entrando 200 unidades, para doze lojas com necessidade QUANTIDADE MININA  de 48 item em cada loja e com QUANTIDADE MAXIMA de 96 em quatro lojas.


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



Resultado esperado, quatro lojas deverá ter um romaneio de transferência, com 48 unidades em cada de acordo com a Prioridade de abastecimento da unidade (loja), e uma loja ira receber 02 itens de acordo com a prioridade.

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
8 | 1
9 | 2
10| 0
11| 0
12| 48
13| 0



### Teste 06 - Item sem encomendas, com o multiplo de 1 item, entrando 2000 unidades, para doze lojas com necessidade QUANTIDADE MININA  de 48 item em cada loja e com QUANTIDADE MAXIMA de 96 em quatro lojas.


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



Resultado esperado, quatro lojas deverá ter um romaneio de transferência, com 48 unidades em cada de acordo com a Prioridade de abastecimento da unidade (loja), e uma loja ira receber 02 itens de acordo com a prioridade.

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
8 | 1
9 | 2
10| 0
11| 0
12| 48
13| 0

Calulando o atendimento mínimo/crítico sobram 3 unidades excedentes.


Calculando a distribuição do excedente:
soma dos giros (1,2,3) => 6
giro ponderado
Loja 1 = 1/6= 0,166
Loja 2 = 2/6 = 0,333
Loja 3 = 3/6= 0,5

Ordena do maior para o menor de arredonda para cima.

Loja 3  3* 0,5= 1,5 arrendondando para cima fica 2
Sobrou 1

Loja 2, tem peso 2/6, 
Recebe mais um excedente.

Para a loja 1, não há mais excedentes.

Saidas
romaneio_item_dbf
LOJA | QUANTIDADE
---: | ---:
1 | 1
2 | 2
3 | 3


### Teste 04 - Item sem encomendas, com o multiplo de 1 item, entrando 60 unidades, para três lojas com necessidade de 2 item em cada loja, com giro discrepante.

Apos atribuir o 

Entradas
produto_estoque
LOJA | QUANTIDADE_MINIMA | QUANTIDADE_CRITICO | ESTOQUE_VIRTUAL | EMPENHADO_ENCOMENDA | GIRO
---: | ---: | ---: | ---: | ---: | ---:
1 | 1 | 2 | 0 | 0 | 1
2 | 1 | 2 | 0 | 0 | 2
3 | 1 | 2 | 0 | 0 | 3

Calulando o atendimento mínimo/crítico sobram 54 unidades excedentes.


Calculando a distribuição do excedente:
soma dos giros (1,2,3) => 6
giro ponderado
Loja 1 = 1/6=0,166
Loja 2 = 2/6=0,333
Loja 3 = 3/6= 0,5

Ordena do maior para o menor de arredonda para cima.

Loja 3 tem peso 3/6
54* 0,5 =>27
Sobrou 27

Loja 2, tem peso 2/6, 
54 * 0,333 => 17,98 arrendondado para cima 18.
Sobrou 9

Para a loja 1
54 * (1/6) => 9, então fica com os 9


Saidas
romaneio_item_dbf
LOJA | QUANTIDADE
---: | ---:
1 | 2 + 9 (11)
2 | 2 + 18 (20)
3 | 2 + 27 (29)


### Teste 05 - Item sem encomendas, com o multiplo de 1 item, entrando 5 unidades, para três lojas com necessidade de 2 item em cada loja.

Apos atribuir o 

Entradas
produto_estoque
LOJA | QUANTIDADE_MINIMA | QUANTIDADE_CRITICO | ESTOQUE_VIRTUAL | EMPENHADO_ENCOMENDA | GIRO
---: | ---: | ---: | ---: | ---: | ---:
1 | 1 | 2 | 0 | 0 | 1
2 | 1 | 2 | 0 | 0 | 2
3 | 1 | 2 | 0 | 0 | 3

lojas
LOJA | PRIORIDADE_ABASTECIMENTO
---: | ---:
1 | 1
2 | 100
3 | 50

Calulando o atendimento mínimo/crítico.

Divide-se as unidades pelo EMBALAGEM_QUANTIDADE_EMPRESA, considerando neste caso o valor 1.
Distribuindo = compra_item.QUANTIDADE_ESTOQUE.

Ordem de abastecimento das unidades pela prioridade = [1,3,2]

Ordenar as lojas por prioridade
Enquanto Distribuindo>0
 distribui um unidade para a loja da vez
 diminui a quantidade distribuindo.
  
Saidas
romaneio_item_dbf
LOJA | PRIORIDADE_ABASTECIMENTO
---: | ---:
1 | 2
2 | 1
3 | 2



### Teste 06 - Item sem encomendas, com o multiplo de 4 itens, entrando 200 unidades, para três lojas, com giro discrepante.


Entradas
produto_estoque
LOJA | QUANTIDADE_MINIMA | QUANTIDADE_CRITICO | ESTOQUE_VIRTUAL | EMPENHADO_ENCOMENDA | GIRO
---: | ---: | ---: | ---: | ---: | ---:
1 | 20 | 0 | 0 | 0 |72
2 | 12 | 0 | 0 | 0 | 30
3 | 16 | 0 | 0 | 0 | 20

Calulando o atendimento mínimo/crídtio sobram 152 unidades excedentes.


Calculando a distribuição do excedente:
soma dos giros (72,30,20) => 122
giro ponderado
Loja 1 = 72/122 = 0,5901
Loja 2 = 30/122= 0,2459
Loja 3 = 20/122 = 0,1639

Loja 1
152 * 0.5901 = 89,69  /4 multiplo    = 22,42  arrendondando 23 *4= 92
Sobrou 80

Loja 2, 
152 * 0.2459 =37,37 /4 multiplo =9,34 arrendondando 10*4=40
Sobrou 40

Para a loja 3
152 * 0,1639 = 24,91/4 multiplo = 6,22 arrendondado 7 *4 = 28


Saidas
romaneio_item_dbf
LOJA | QUANTIDADE
---: | ---:
1 | 20 + 92 (112)
2 | 12 + 40 (52)
3 | 16 + 20 (36)

### Teste 07 - Item sem encomendas, com o multiplo de 4 item, entrando  unidades 20, para três lojas com necessidade de 60 item em cada loja.


Entradas
produto_estoque
LOJA | QUANTIDADE_MINIMA | QUANTIDADE_CRITICO | ESTOQUE_VIRTUAL | EMPENHADO_ENCOMENDA | GIRO
---: | ---: | ---: | ---: | ---: | ---:
1 | 4 | 20 | 0 | 0 |10
2 | 4 | 20 | 0 | 0 |20
3 | 4 | 20 | 0 | 0 |30

lojas
LOJA | PRIORIDADE_ABASTECIMENTO
---: | ---:
1 | 1
2 | 100
3 | 50

Calulando o atendimento mínimo/crítico.

Divide-se as unidades pelo EMBALAGEM_QUANTIDADE_EMPRESA, considerando neste caso o valor 4.
Distribuindo = compra_item.QUANTIDADE_ESTOQUE.

Ordem de abastecimento das unidades pela prioridade = [1,3,2]

Ordenar as lojas por prioridade
Enquanto Distribuindo>0
 distribui quatro unidade para a loja da vez
 diminui a quantidade distribuindo.

Saidas
romaneio_item_dbf
LOJA | PRIORIDADE_ABASTECIMENTO
---: | ---:
1 | 2
2 | 1
3 | 2


### ### Teste 08 - Item com encomenda, com o multiplo de 1 item, entrando 6 unidades, para cinco lojas com necessidade de 1 item em cada loja.

produto_estoque
LOJA | QUANTIDADE_MINIMA | QUANTIDADE_CRITICO | ESTOQUE_VIRTUAL | EMPENHADO_ENCOMENDA | GIRO
---: | ---: | ---: | ---: | ---: | ---:
1 | 1 | 0 | 0 | 0 | 0
2 | 1 | 0 | 0 | 0 | 0
3 | 1 | 0 | 0 | 0 | 0
4 | 1 | 0 | 0 | 1 | 0
5 | 1 | 0 | 0 | 0 | 0

Resultado esperado, cada loja deverá ter um romaneio de transferência, com 1 unidades em cada, sendo a que tem a loja que tem empenhado_econcomenda recebe 2 romaneios com um item em cada.

romaneio_item_dbf
LOJA | QUANTIDADE
---: | ---:
1 | 1
2 | 1
3 | 1
4 | 2
5 | 1




### ### Teste 09 - Item com uma encomenda, com o multiplo de 1 item, entrando 3 unidades, para cinco lojas com necessidade de 1 item em cada loja.

produto_estoque
LOJA | QUANTIDADE_MINIMA | QUANTIDADE_CRITICO | ESTOQUE_VIRTUAL | EMPENHADO_ENCOMENDA | GIRO
---: | ---: | ---: | ---: | ---: | ---:
1 | 1 | 0 | 0 | 0 | 0
2 | 1 | 0 | 0 | 0 | 0
3 | 1 | 0 | 0 | 0 | 0
4 | 1 | 0 | 0 | 1 | 0
5 | 1 | 0 | 0 | 0 | 0

lojas
LOJA | PRIORIDADE_ABASTECIMENTO
---: | ---:
1 | 1
2 | 100
3 | 200
4 | 30
5 | 50


Calulando o atendimento mínimo/crítico.

Divide-se as unidades pelo EMBALAGEM_QUANTIDADE_EMPRESA, considerando neste caso o valor 1.
Distribuindo = compra_item.QUANTIDADE_ESTOQUE.

Ordem de abastecimento das unidades pela prioridade se altera levando em consideração em qual loja tem empenhado_encomenda = [4,1,2,5,3]

Ordenar as lojas por prioridade
Enquanto Distribuindo>0
A loja que tem empenhado_encomenda tem a primeira prioridade,  seguindo para a proxima loja da vez seguindo o criterio de prioridade.
 diminui a quantidade distribuindo.

Saidas
romaneio_item_dbf
LOJA | PRIORIDADE_ABASTECIMENTO
---: | ---:
1 | 1
2 | 0
3 | 0
4 | 1
5 | 1

### ### Teste 10 - Item com duas encomenda, com o multiplo de 1 item, entrando 3 unidades, para cinco lojas com necessidade de 1 item em cada loja.

produto_estoque
LOJA | QUANTIDADE_MINIMA | QUANTIDADE_CRITICO | ESTOQUE_VIRTUAL | EMPENHADO_ENCOMENDA | GIRO
---: | ---: | ---: | ---: | ---: | ---:
1 | 1 | 0 | 0 | 0 | 0
2 | 1 | 0 | 0 | 1 | 0
3 | 1 | 0 | 0 | 0 | 0
4 | 1 | 0 | 0 | 1 | 0
5 | 1 | 0 | 0 | 0 | 0

lojas
LOJA | PRIORIDADE_ABASTECIMENTO
---: | ---:
1 | 1
2 | 100
3 | 200
4 | 30
5 | 50


Calulando o atendimento mínimo/crítico.

Divide-se as unidades pelo EMBALAGEM_QUANTIDADE_EMPRESA, considerando neste caso o valor 1.
Distribuindo = compra_item.QUANTIDADE_ESTOQUE.

Ordem de abastecimento das unidades pela prioridade se altera levando em consideração em qual lojas tem empenhado_encomenda  = [4,2,1,5,3]

Ordenar as lojas por prioridade
Enquanto Distribuindo>0
A lojas que tem empenhado_encomenda tem a primeira prioridade,  seguindo para a proxima loja da vez seguindo o criterio de prioridade.
 diminui a quantidade distribuindo.

Saidas
romaneio_item_dbf
LOJA | PRIORIDADE_ABASTECIMENTO
---: | ---:
1 | 1
2 | 1
3 | 0
4 | 1
5 | 0

### ### Teste 11 - Item com quatro encomendas, com o multiplo de 1 item, entrando 3 unidades, para cinco lojas com necessidade de 1 item em cada loja.

produto_estoque
LOJA | QUANTIDADE_MINIMA | QUANTIDADE_CRITICO | ESTOQUE_VIRTUAL | EMPENHADO_ENCOMENDA | GIRO
---: | ---: | ---: | ---: | ---: | ---:
1 | 1 | 0 | 0 | 0 | 0
2 | 1 | 0 | 0 | 1 | 0
3 | 1 | 0 | 0 | 1 | 0
4 | 1 | 0 | 0 | 1 | 0
5 | 1 | 0 | 0 | 1 | 0



LOJA | PRIORIDADE_ABASTECIMENTO
---: | ---:
1 | 1
2 | 100
3 | 200
4 | 30
5 | 50

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

Loja 4- empenhado_encomenda 10/10/2022 as 10:30- recebe na proxima entrada como prioridade

Loja 5- empenhado_encomenda 10/10/2022 as 8:30 - recebe 01 


Saidas
romaneio_item_dbf
LOJA | PRIORIDADE_ABASTECIMENTO
---: | ---:
1 | 0
2 | 1
3 | 1
4 | 0
5 | 1

### Teste 12 - O mesmo produto aparecendo em mais de uma linha da compra.
### Teste 13 - O calculo estava ficando neativo ou 0 por conta de comprado...
 que é >0 e encomenda>0, 
 Entrando 1, comprado 3, disponivel 0,, encomenda 1, qtdMinima 1, estoqueVirutalLocalDescontad 3, demandaLocal ficava -1,  nese caso apresentava erro na redistribuicao
 e nao deixava cadastrar, ou se o caso da demanda local ficasse 0 nao atendia encomenda.

 ### Testar duas compras com o mesmo item entrando ao memso tempo, pode haver um de encomenda e outra de fabricante.

 



