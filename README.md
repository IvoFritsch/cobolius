# Cobolius

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
### Numeros ###
Os numeros são declarados da mesma forma que no C, simplesmente escrevendo ele da forma decimal:
```
DEFINE preco COM 19.99.
```
### Strings ###
As strings são declaradas da mesma forma que no Javascript, entre `'` ou `"`:
```
DEFINE nome COM 'Ivo Fritsch'.
```
### Listas ###
As listas são imutaveis, indexadas a partir de e são declaradas com a palavra `LISTA`:
```
DEFINE minhaListinha COM LISTA.
DEFINE CONSTANTE digitos COM LISTA DE 0 A 9.
DEFINE nomes COM LISTA DE 'Ivo', 'Pedro', 'Paulo'.
```
Como as listas são imutáveis, todas as operações em listas retornam uma nova lista:

```
// Adicionar 'Anna' no final da lista:
nomes = nomes + 'Anna'.
// Remover 3 itens do final da lista:
nomes = nomes - 3.
// Remover 3 itens do inicio da lista:
nomes = 3 - nomes.
// Adicionar 'José' no inicio da lista:
nomes = 'Jose' + nomes.
// Obter elemento 2 da lista
DEFINE pessoa COM nomes[2].
```


### Parágrafos ###

Um conjunto de frases em Cobolius compõe um parágrafo, um parágrafo é o equivalente a uma função em c, a definição de um parágrafo é feita da seguinte forma:
```
COMPOE <nome> RECEBENDO <param_1>  [, | E]  <param_3> [, | E] <param_3>:
   ...<frases>
```
A definição de um parágrafo somente termina no final do arquivo fonte ou na definição de um novo parágrafo.

Um parágrafo pode retornar um valor com a frase `RETORNA <valor>.`

A chamada de um parágrafo é feita diretamente passando o nome do parâgrafo seguido pelos parâmetros entre parênteses separados por `,` ou `E`:
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

### Exemplo ###

Segue um exemplo de um programa recursivo que exibe o fatorial de um número:

```
COMPOE fatorial RECEBENDO numero:
   SE numero < 1 RETORNA 1.
   RETORNA numero * fatorial(numero - 1).
   
COMPOE INICIO:
   printaNoConsole(fatorial(13)).
```

