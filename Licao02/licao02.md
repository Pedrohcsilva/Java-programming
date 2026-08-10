---
title: "Programação em Java"
subtitle: "Lição 2: Repetição e Métodos"
author: "Java Programming MOOC · Universidade de Helsinque — tradução para o português"
date: "2026"
lang: pt-BR
---

# Lição 2: Repetição e Métodos

## Objetivos de aprendizagem

Ao final desta lição, você será capaz de:

- Usar o laço `while` para repetir código
- Usar os comandos `break` e `continue`
- Usar o laço `for` para percorrer sequências
- Criar e chamar métodos próprios
- Criar métodos com parâmetros e com valor de retorno

## Padrões comuns de repetição

Antes de aprender os laços, é útil reconhecer padrões frequentes. Dois dos mais comuns são:

**Leitura contínua até um valor sentinela:**
```java
Scanner scanner = new Scanner(System.in);
while (true) {
    String linha = scanner.nextLine();
    if (linha.equals("fim")) {
        break;
    }
    System.out.println("Você digitou: " + linha);
}
```

**Acúmulo de resultado:**
```java
int soma = 0;
int contador = 0;
// ... leitura de valores no laço ...
double media = (double) soma / contador;
```

## O laço `while`

O laço `while` repete um bloco de código enquanto uma condição for verdadeira.

### Forma básica com condição

```java
int numero = 1;
while (numero <= 5) {
    System.out.println(numero);
    numero++;
}
```

**Saída:**
```
1
2
3
4
5
```

### Laço infinito com `break`

Muitas vezes é mais claro usar `while (true)` e sair com `break`:

```java
Scanner scanner = new Scanner(System.in);

int soma = 0;
while (true) {
    System.out.print("Digite um número (0 para sair): ");
    int n = Integer.valueOf(scanner.nextLine());
    if (n == 0) {
        break;
    }
    soma += n;
}
System.out.println("Soma total: " + soma);
```

### O comando `continue`

O `continue` pula o restante da iteração atual e volta para o início do laço:

```java
int i = 0;
while (i < 10) {
    i++;
    if (i % 2 == 0) {
        continue; // pula os pares
    }
    System.out.println(i); // imprime apenas ímpares: 1, 3, 5, 7, 9
}
```

### Calculando estatísticas com laço

Variáveis de acúmulo devem ser declaradas **antes** do laço:

```java
Scanner scanner = new Scanner(System.in);

int soma = 0;
int quantidade = 0;
int maior = Integer.MIN_VALUE;

while (true) {
    System.out.print("Digite um número (ou -1 para encerrar): ");
    int n = Integer.valueOf(scanner.nextLine());
    if (n == -1) {
        break;
    }
    soma += n;
    quantidade++;
    if (n > maior) {
        maior = n;
    }
}

if (quantidade > 0) {
    System.out.println("Quantidade: " + quantidade);
    System.out.println("Soma: " + soma);
    System.out.println("Média: " + (double) soma / quantidade);
    System.out.println("Maior: " + maior);
}
```

## O laço `for`

O `for` é mais compacto e muito usado quando se sabe antecipadamente quantas iterações serão feitas.

### Sintaxe

```java
for (inicialização; condição; atualização) {
    // corpo
}
```

### Exemplos

```java
// Conta de 1 a 5
for (int i = 1; i <= 5; i++) {
    System.out.println(i);
}

// Conta de 10 a 1 (regressivo)
for (int i = 10; i >= 1; i--) {
    System.out.println(i);
}

// Pulos de 2 em 2
for (int i = 0; i <= 10; i += 2) {
    System.out.println(i); // 0 2 4 6 8 10
}
```

### Laços aninhados

```java
for (int linha = 1; linha <= 3; linha++) {
    for (int coluna = 1; coluna <= 3; coluna++) {
        System.out.print(linha + "x" + coluna + "=" + (linha * coluna) + "  ");
    }
    System.out.println();
}
```

**Saída:**
```
1x1=1  1x2=2  1x3=3  
2x1=2  2x2=4  2x3=6  
3x1=3  3x2=6  3x3=9  
```

## Métodos

Um **método** é um bloco de código nomeado que realiza uma tarefa específica. Métodos evitam repetição e tornam o programa mais legível e organizado.

### Método sem parâmetros e sem retorno

```java
public static void saudacao() {
    System.out.println("Olá! Bem-vindo ao programa.");
}
```

Chamada:

```java
public static void main(String[] args) {
    saudacao();
    saudacao();
}
```

**Saída:**
```
Olá! Bem-vindo ao programa.
Olá! Bem-vindo ao programa.
```

### Método com parâmetros

Parâmetros permitem que o método receba informações externas:

```java
public static void saudar(int vezes) {
    for (int i = 0; i < vezes; i++) {
        System.out.println("Olá!");
    }
}

public static void soma(int a, int b) {
    System.out.println(a + " + " + b + " = " + (a + b));
}
```

Chamadas:

```java
saudar(3);     // imprime "Olá!" três vezes
soma(5, 7);    // imprime "5 + 7 = 12"
```

> **Atenção:** Parâmetros são **cópias** dos valores passados. Modificá-los dentro do método não altera as variáveis originais.

### Método com valor de retorno

Substitua `void` pelo tipo do valor retornado e use `return`:

```java
public static int soma(int a, int b) {
    return a + b;
}

public static double media(int a, int b, int c) {
    return (a + b + c) / 3.0;
}

public static boolean ehPar(int n) {
    return n % 2 == 0;
}
```

Usando o retorno:

```java
int resultado = soma(3, 4);
System.out.println(resultado); // 7

double m = media(10, 20, 30);
System.out.println(m); // 20.0

if (ehPar(8)) {
    System.out.println("É par!");
}
```

### Exemplo completo com métodos

```java
import java.util.Scanner;

public class Calculadora {

    public static double calcularMedia(int a, int b, int c) {
        return (a + b + c) / 3.0;
    }

    public static int calcularMaximo(int a, int b) {
        if (a > b) {
            return a;
        }
        return b;
    }

    public static void exibirResultados(double media, int maximo) {
        System.out.println("Média: " + media);
        System.out.println("Maior dos dois primeiros: " + maximo);
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Primeiro número: ");
        int n1 = Integer.valueOf(scanner.nextLine());
        System.out.print("Segundo número: ");
        int n2 = Integer.valueOf(scanner.nextLine());
        System.out.print("Terceiro número: ");
        int n3 = Integer.valueOf(scanner.nextLine());

        double media = calcularMedia(n1, n2, n3);
        int maximo = calcularMaximo(n1, n2);

        exibirResultados(media, maximo);
    }
}
```

### Escopo de variáveis

Variáveis definidas dentro de um método existem apenas naquele método. Você não pode acessá-las de fora:

```java
public static void exemplo() {
    int x = 10; // x só existe aqui dentro
}

public static void main(String[] args) {
    exemplo();
    // System.out.println(x); // ERRO: x não existe aqui
}
```

## Quando usar `while` vs. `for`

| Situação | Use |
|:---------|:----|
| Número de iterações desconhecido | `while` |
| Lendo até um valor sentinela | `while (true)` com `break` |
| Percorrendo um intervalo conhecido | `for` |
| Percorrendo uma coleção | `for-each` (veremos em breve) |

## Resumo

- O laço `while` repete enquanto a condição for verdadeira; `break` sai do laço, `continue` pula a iteração.
- O laço `for` é ideal quando sabemos o número de iterações.
- Variáveis de acúmulo devem ser declaradas **antes** do laço.
- Um **método** é um bloco nomeado de código que pode receber parâmetros e retornar valores.
- Parâmetros são cópias: alterá-los no método não afeta as variáveis originais.
- `return` encerra o método e devolve um valor.

## Exercícios

**Exercício 1 — Tabuada**

Escreva um programa que lê um número inteiro `n` e imprime a tabuada de `n` (de 1 a 10).

**Exercício 2 — Fatorial**

Crie um método `fatorial(int n)` que retorna o fatorial de `n`. Teste com `fatorial(0)`, `fatorial(1)`, `fatorial(5)` e `fatorial(10)`.

**Exercício 3 — Contagem de pares e ímpares**

Escreva um programa que lê números inteiros até o usuário digitar `0`, e ao final exibe quantos foram pares e quantos foram ímpares.

**Exercício 4 — Potência**

Crie um método `potencia(int base, int expoente)` que calcula `base` elevado ao `expoente` usando um laço (sem usar `Math.pow`). Teste com `potencia(2, 10)`.

**Exercício 5 — Validação de entrada**

Crie um método `lerInteiroPositivo(Scanner scanner)` que:
- Pede ao usuário para digitar um número positivo
- Se o número for zero ou negativo, exibe uma mensagem de erro e repete
- Retorna o número somente quando for positivo

Use esse método no `main` para ler dois números e exibir o produto.

**Exercício 6 — Estrelas**

Crie um método `imprimirEstrelas(int n)` que imprime uma linha com `n` asteriscos. Depois, no `main`, use-o para imprimir um triângulo:

```
*
**
***
****
*****
```
