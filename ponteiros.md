# Ponteiros

### Calma, calma, calma! Eu sei que vc odeia ele... mas não _desaponte_ o coitado do _ponteiro_ (tá até destacada a piada :D)

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

Trabalhar com funções implica diretamente em manipular seus parâmetros.
