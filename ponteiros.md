# Ponteiros

### Calma, calma, calma! Eu sei que vc odeia ele... mas não _desaponte_ o _ponteiro_ (tá até destacada a piada :D)

- Variáveis comuns
- Problemas com variáveis comuns
- Intuição de ponteiros
- Ponteiros em C

## Variáveis comuns

Vamos começar entendendo (ou recordando) como variáveis comuns se comportam na linguagem C, posteriormente vamos analisar algumas situações em que utilizar apenas variáveis comuns (que não são ponteiros) traz desvantagens em performance, legibilidade de código e/ou otimização de memória.

Para começar, daqui pra frente vamos relacionar o conceito de variável com o de uma gaveta, ela é um espaço que vamos utilizar para armazenar algo, no caso da computação vamos armazenar dados (números, textos, booleans...).

Em um grande gaveteiro é interessante colocar alguma identificação nas gavetas para saber seu conteúdo. Na programação fazemos o mesmo. A memória do computador é um gigantesco gaveteiro e cada gaveta (variável) recebe um nome:

```c
int minha_gavetinha;
```

Com isso separamos uma das gavetas para armazenar um valor inteiro, agora basta inserir algo dentro dela:

```c
int minha_gavetinha;
minha_gavetinha = 10;
```

Agora, o conteúdo guardado dentro de `minha_gavetinha` é o valor 10.

Guarde bem essa informação, pois será importante para entender ponteiros posteriormente: a variável `minha_gavetinha`, como uma gaveta real, armazena exatamente o conteúdo que guardamos nela (nesse caso o valor 10). Parece besta, mas é importante.

### Funções

> Se tem dúvidas sobre funções dê uma olhada no material sobre funções, senão vai boiar aqui :)

#### Parâmetros

Trabalhar com funções implica diretamente em manipular seus parâmetros e eles, assim como as variáveis, podem ser ponteiros ou não.

Quando um parâmetro ***não é*** um ponteiro o valor que enviamos pra ele é na verdade uma cópia do valor original.

Ex.: Ao fazer matrícula na escola levamos à secretaria cópias de nossos documentos originais. Então, a escola pode fazer o que quiser com a cópia sem comprometer nosso original.

Vamos visualizar esse comportamente em um código simples:

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

Vamos entender o que aconteceu...

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
