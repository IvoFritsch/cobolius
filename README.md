
# Linguagem Cobolius #

## Introdução ##

Este manual de referência descreve a linguagem de programação Cobolius.

### Motivação e Contexto ###

A linguagem Cobolius tem como base a antiga linguagem Cobol, com elementos de Javascript, que já foi muito utilizada em todo o mundo para construir sistema cuja confiabilidade é um fator crítico, como em sistemas bancários e financeiros.

### Notação ###

Na linguagem Cobolius, todas as palavras reservadas são *MAIÚSCULAS* enquanto as variáveis são *case-sensitive* iniciando com *minusculas* ou "_", a notação das variáveis é a seguinte:

```
<nome>  ::= (<minúscula> | "_") (<letra> | "_" | <numero>)*
<letra> ::=  "a"..."z" | "A"..."Z"
<minuscula> ::=  "a"..."z"
<numero> ::=  "0"..."9"
```
São exemplos de variáveis válidas:
- _contador
- contador3
- contador_4

São exemplos de variáveis inválidas:
- 3contador
- 1234
- VAR
- minha-variavel

## Frases ##

Na linguagem Cobolius os comandos são chamados de frases e cada frase deve ser terminada por um `.`.

## Identação ##

Na linguagem Cobolius a identação é irrelevante.

## Comentarios ##

Na linguagem Cobolius os comentarios iniciam-se com `//` e vão até o final da linha

## Variáveis ##
Em Cobolius, as variáveis não são previamente tipadas, e tem seu tipo definido no momento da atribuição

As variáveis podem ser definidas como constantes no momento da sua declaração

A definição de variáveis é feita com a uma frase iniciando com `DEFINE`, e mais de uma variavel pode ser definida por frase:
```
DEFINE ([CONSTANTE] <nome> [COM <valor_inicial>][, | E])*.
```
Por exemplo, para definirmos um número constante e uma string:
```
DEFINE CONSTANTE meuNumero COM 4 E minhaString com 'Ivo Fritsch'.
```
A atribuição de uma variável  é feita mesma forma que no C, com o `=`:
```
DEFINE meuNumero COM 4.
meuNumero = 4.
```
### Números ###
Os números são declarados da mesma forma que no C, simplesmente escrevendo ele da forma decimal:
```
DEFINE preco COM 19.99.
```
### Strings ###
As strings são declaradas da mesma forma que no Javascript, entre `'` ou `"`:
```
DEFINE nome COM 'Ivo Fritsch'.
```
### Listas ###
As listas são diversas variáveis indexadas a partir de 0 e são declaradas com a palavra `LISTA`:
```
DEFINE minhaListinha COM LISTA.
DEFINE CONSTANTE digitos COM LISTA DE 0 A 9.
DEFINE nomes COM LISTA DE 'Ivo', 'Pedro', 'Paulo'.
```
As operações em listas são feitas com os sinais de `-` e `+`:
```
// Adicionar 'Anna' no final da lista:
nomes + 'Anna'.

// Remover 3 itens do final da lista:
// A remoção de itens retorna os itens removidos como uma nova lista e 
//      altera a lista alvo da operação
nomesRemovidos = nomes - 3.

// Remover 3 itens do inicio da lista:
nomesRemovidos = 3 - nomes.

// Adicionar 'José' no inicio da lista:
'Jose' + nomes.

// Obter elemento 2 da lista
pessoa = nomes[2].

// Atribui elemento 2 da lista
nomes[2] = 'Nome dois'.

// Pode-se adicionar diversas variáveis em uma lista de uma só vez:
nomes + 'Nome 1', 'Nome 2', 'Nome 3', 'Nome 4'.

// Pode-se extrair os itens de uma lista para variáveis passando opcionalmente o item 
// inicial e final:
EXTRAI nomes PARA nome1, nome2, nome3. // <- Extrai os tres primeiros itens para as variaveis
DEFINE novaLista COM EXTRAI nomes DE 0 A 1.
// Pega a quantidade de elementos em uma lista
tamanho = nomes.qtd.

// A concatenção de listas pode ser feita com uma combinação dos comandos EXTRAI 
// e com a adição de elementos:
lista1 = LISTA DE 0 A 5.
lista2 = LISTA DE 10 A 100.
lista1 + EXTRAI lista2.
printaNoConsole(lista1). // <- 0, 1, 2, 3, 4, 5, 10, 11, 12, 13 ....
```


### Parágrafos ###

Um conjunto de frases em Cobolius compõe um parágrafo, um parágrafo é o equivalente a uma função em c, a definição de um parágrafo é feita da seguinte forma:
```
COMPOE <nome> RECEBENDO <param_1>  [, | E]  <param_3> [, | E] <param_3>:
   ...<frases>
```
A definição de um parágrafo somente termina no final do arquivo fonte ou na definição de um novo parágrafo.

Um parágrafo pode retornar um valor com a frase `RETORNA <valor>.`

A chamada de um parágrafo é feita diretamente passando o nome do parágrafo seguido pelos parâmetros entre parênteses separados por `,` ou `E`:
```
COMPOE soma RECEBENDO a E b:
   RETORNA a + b.
// "INICIO" é uma palavra reservada para o parágrafo que é o ponto de partide da execução do programa
COMPOE INICIO:
   DEFINE idade COM 15.
   printaNoConsole(soma(13 E idade)).
```
### Condições ###
Condições são feitas com a palavra `SE` e terminadas com `FIM`:
```
    SE <condicao>:
        frases...
    [SENAO:
        frases....]
    FIM.
```

```
SE a > b:
   RETORNA a - b.
SENAO:
   RETORNA b - a.
```
## Condições frase-unica ##
Condições frase-unica se caracterizam pela falta do `:` e omitem o `FIM` pois ele é implicito após a primeira frase:
```
    SE <condicao> <frase>.
```
### Laços ###

Todos os tipos de laços são definidos iniciando-se com `REPETE` e terminando com "FIM" e opcionalmente palavras para definir comportamentos extras do laço de repetição:

```
REPETE [[PARA CADA ITEM (EM | NA) <lista> [RECEBENDO <item_iteracao>[, <indice_iteracao>]]]]:
   ...<frases>
FIM.
```

Por exemplo, para repetir um laço por 10 vezes, exibindo o numero da iteração, podemos fazer da seguinte forma:
```
REPETE PARA CADA ITEM NA LISTA DE 1 A 10 RECEBENDO index:
   printaNoConsole (index).
FIM.
```

Exemplo, para exibir o nome das pessoas em uma lista
```
REPETE PARA CADA ITEM EM listaDePessoas RECEBENDO pessoa, index:
   printaNoConsole (index, '->', pessoa.nome.)
FIM.
```

### Exemplos ###

Segue um exemplo de um programa recursivo que exibe o fatorial de um número:

```
COMPOE fatorial RECEBENDO numero:
   SE numero < 1 RETORNA 1.
   RETORNA numero * fatorial(numero - 1).
   
COMPOE INICIO:
   printaNoConsole(fatorial(13)).
```

Exemplo de bubble-sort recursivo para ordenação de uma lista:

```
COMPOE bubbleSort RECEBENDO lista E n: 
    SE n < 1 RETORNA.
	REPETE PARA CADA ITEM NA LISTA DE 0 A n RECEBENDO index:
		SE lista[index] > lista[index + 1]:
		    DEFINE temp COM lista[index]; 
		    lista[index] = lista[index + 1]; 
		    lista[index + 1] = temp; 
		FIM.
	FIM.
    bubbleSort(lista, n-1); 
}

COMPOE INICIO:
	DEFINE listaOrdenar COM LISTA DE 3,5,1,4,1,2,3,54,8,9,4,7,3 E 254.
	bubbleSort(listaOrdenar, listaOrdenar.qtd).
```
