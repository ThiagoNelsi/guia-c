# Ponteiros

### Calma, calma, calma! Eu sei que vc odeia ele... mas não _desaponte_ o _ponteiro_ (tá até destacada a piada :D)

- Variáveis comuns
- Problemas com variáveis comuns
- Intuição de ponteiros
- Ponteiros em C

## Variáveis comuns

Vamos começar entendendo (ou recordando) como variáveis comuns se comportam na linguagem C, posteriormente vamos analisar algumas situações em que utilizar apenas variáveis comuns (que não são ponteiros) pode trazer desvantagens em performance, legibilidade de código e/ou otimização de memória.

Daqui pra frente vamos relacionar o conceito de variável com o de uma gaveta, ela é um espaço que vamos utilizar para armazenar algo, no caso da computação vamos armazenar dados (números, textos, booleans...).

Pense na memória do computador como um grande gaveteiro. Para saber o que cada gaveta guarda é interessante colocar alguma identificação nas gavetas para saber seu conteúdo. Na programação fazemos o mesmo, cada gaveta (variável) recebe um nome:

```c
int minha_gavetinha;
```

Com isso, separamos uma das gavetas para armazenar um valor inteiro, agora basta inserir algo dentro dela:

```c
int minha_gavetinha;
minha_gavetinha = 10;
```

ou 

```c
int minha_gavetinha = 10;
```

Agora, o conteúdo armazenado dentro de `minha_gavetinha` é o valor 10.

Guarde bem essa informação, pois será importante para entender ponteiros posteriormente: a variável `minha_gavetinha`, como uma gaveta real, armazena exatamente o conteúdo que guardamos nela (nesse caso o valor 10). Parece besta, mas é importante.

### Funções

#### Parâmetros

Trabalhar com funções implica diretamente em manipular seus parâmetros e eles, assim como as variáveis, podem ser ponteiros ou não.

Quando um parâmetro ***não é*** um ponteiro o valor que enviamos pra ele é na verdade uma cópia do valor original.

Ex.: Ao fazer matrícula na escola levamos à secretaria cópias de nossos documentos originais. Então, a escola pode fazer o que quiser com a cópia sem comprometer nosso original.

Vamos analisar uma situação análoga em um código simples:

```c
#include <stdio.h>

void dobro(int n) {
  n = n * 2;
  printf("O dobro é %d\n", n);
}

int main() {
  int x = 10;
  printf("O valor de x é %d\n", x);
  dobro(x);
  printf("Após executar a função o valor de x é: %d\n", x);
}
```

A saída será:

```
O valor de x é 10
O dobro é 20
Após executar a função o valor de x é 10
```

Bora entender o que aconteceu...

1. Na função `main` declaramos a variável `x` recebendo o valor 10.
2. Fazemos um printf mostrando o valor de `x` => `O valor de x é 10` 
3. Chamamos a função `dobro`, passando a variável `x` como argumento => `dobro(x);`

Esse ponto é importante! Pense na variável `x` como uma gaveta que guarda um papel com o valor 10 escrito. Ao chamar a função, abrimos a gaveta, pegamos o papel, fazemos um xerox, guardamos o original de volta na gaveta e entregamos a cópia para a função.

4. A função recebe o valor 10 no parâmetro `n` (que podemos visualizar como uma gaveta também).
5. Multiplica-se o valor recebido (10) por 2 e colocamos o novo valor na "gaveta" `n`. => `n = n * 2;`
6. A função executa o printf com o valor de `n` => `O dobro é 20`
7. O programa volta para a função main e executa outro printf com o valor de `x` => `Após executar a função o valor de x é 10`

Mas por que não saiu 20?

R: Pois enviamos uma cópia do valor de x para a função `dobro`. A cópia foi alterada pela função, mas o valor orignal permaneceu armazenado na variável `x`.

Se quisermos utilizar a função dobro para atualizar o valor de x podemos fazer:

```c
#include <stdio.h>

int dobro(int n) {
  n = n * 2;
  printf("O dobro é %d\n", n);]
  return n;
}

int main() {
  int x = 10;
  printf("O valor de x é %d\n", x);
  x = dobro(x);
  printf("Após executar a função o valor de x é: %d\n", x);
}
```

A saída será:

```
O valor de x é 10
O dobro é 20
Após executar a função o valor de x é 20
```

Agora estamos dizendo que a função `dobro` irá retornar o valor atualizado de n. Antes, ela apenas atualizava o valor de `n`, fazia o printf e jogava `n` no lixo. Após as modificações, a função faz a atualização no valor de `n`, executa o printf e devolve o valor atualizado para quem chamou a função. Então, pegamos esse valor atualizado que a função retorna e guardamos dentro da gaveta `x`.

`x = dobro(x);`


## Problemas com variáveis comuns

Tudo o que foi dito antes funciona muito bem em cenários simples, em que trabalhamos com números e caracteres únicos. Que são contúedos que "cabem na gaveta" e que podem ser copiados para outras variáveis/funções de forma rápida e fácil, computacionalmente falando.

Entretanto, para outros valores, como strings, a gaveta pode não ser grande o suficiente. Além disso, o processo de fazer uma cópia do valor para enviar para funções pode ser muito custoso para o computador.

Mas como assim a gaveta pode não ser suficientemente grande? 

Bom, a resposta está em dois carinhas: stack e heap, mas relaxa! Abre o coração que vai ser rápido.

A memória do programa se divide em stack e heap. *Stack* significa "pilha" e *heap* "amontoado". Enquanto os dados salvos na *stack* ficam organizados linearmente (em pilha), os dados no *heap* não ficam distribuídos de forma organizada. 

Mas qual a vantagem do *heap* então? Entre outras razões, podemos dizer que uma das principais vantagens do heap é que o espaço da memória disponível para nosso programa utilizar pode aumentar dinamicamente, enquanto a stack tem tamanho fixo.

Para entender melhor a vantagem disso imagine um programa que lê arquivos, armazena seu conteúdo em memória e conta quantas linhas ele possui. Inicialmente seu programa não sabe qual será o tamanho do arquivo, então ele precisa de um espaço grande ou que possa aumentar de tamanho conforme necessário. Esse espaço é o *heap*! Sabendo que a *stack* é limitada, esse tipo de dado, sem tamanho definido ou muito grande deve ser armazenado no *heap*.

Mas existem outros problemas, imagine uma situação em que precisamos de uma variável para armazenar um grande texto que será usado em nosso programa. Então, em determinado ponto, enviamos esse texto para uma função que irá contar quantas letras A existem no texto. O processo de clonar esse texto para enviar para a função (como explicado anteriormente) seria um desperdício enorme de memória e processamento.

Já conhecemos o problema, mas como solucionar?

## Intuição de ponteiros

## Ponteiros em C
