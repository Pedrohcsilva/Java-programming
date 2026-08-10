---
title: "Programação em Java"
subtitle: "Lição 1: Primeiros Passos"
author: "Java Programming MOOC · Universidade de Helsinque — tradução para o português"
date: "2026"
lang: pt-BR
---

# Lição 1: Primeiros Passos

## Objetivos de aprendizagem

Ao final desta lição, você será capaz de:

- Entender o que é um programa de computador
- Escrever programas que exibem texto na tela
- Ler dados digitados pelo usuário
- Criar e usar variáveis dos tipos `String`, `int`, `double` e `boolean`
- Realizar cálculos aritméticos básicos
- Usar condicionais (`if`, `else if`, `else`)

## O que é programação?

Programação é a arte de escrever instruções que um computador possa executar. Essas instruções são chamadas de **código**, e um conjunto organizado delas forma um **programa**.

Os computadores são máquinas que executam instruções com extrema rapidez, mas são completamente literais: fazem exatamente o que o programador manda, nem mais, nem menos. Por isso, escrever código envolve ser muito preciso.

Java é uma das linguagens de programação mais utilizadas no mundo. É uma linguagem **compilada**, **fortemente tipada** e **orientada a objetos**. Neste curso, aprenderemos Java do zero.

## Estrutura de um programa Java

Todo programa Java tem a mesma estrutura básica:

```java
public class NomeDoPrograma {
    public static void main(String[] args) {
        // o código do programa vai aqui
    }
}
```

- `public class NomeDoPrograma` define a **classe** do programa. O nome da classe deve coincidir com o nome do arquivo (ex.: `NomeDoPrograma.java`).
- `public static void main(String[] args)` é o **método principal** — o ponto de partida da execução.
- As chaves `{ }` delimitam blocos de código.
- O `//` inicia um comentário, que é ignorado pelo compilador.

## Exibindo texto na tela

O comando mais básico de Java é o `System.out.println`, que imprime uma linha de texto:

```java
public class OlaMundo {
    public static void main(String[] args) {
        System.out.println("Olá, mundo!");
    }
}
```

**Saída:**
```
Olá, mundo!
```

Você pode imprimir várias linhas repetindo o comando:

```java
System.out.println("Primeira linha");
System.out.println("Segunda linha");
System.out.println("Terceira linha");
```

**Saída:**
```
Primeira linha
Segunda linha
Terceira linha
```

Para imprimir sem pular a linha, use `System.out.print`:

```java
System.out.print("Olá, ");
System.out.print("mundo!");
```

**Saída:**
```
Olá, mundo!
```

> **Dica:** Na maioria das IDEs, você pode digitar `sout` e pressionar Tab para escrever `System.out.println()` automaticamente.

## Lendo a entrada do usuário

Para ler dados digitados pelo usuário, usamos a classe `Scanner`:

```java
import java.util.Scanner;

public class LeitorDeNome {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Qual é o seu nome? ");
        String nome = scanner.nextLine();

        System.out.println("Olá, " + nome + "!");
    }
}
```

**Execução:**
```
Qual é o seu nome? Maria
Olá, Maria!
```

O `import java.util.Scanner` torna a classe `Scanner` disponível. O `scanner.nextLine()` lê uma linha inteira de texto digitada pelo usuário.

## Variáveis

Uma **variável** é um espaço nomeado na memória onde guardamos um valor. Em Java, toda variável tem um **tipo** definido na criação.

### Tipos básicos

| Tipo | Descrição | Exemplo |
|:-----|:----------|:--------|
| `String` | Texto | `"Olá"` |
| `int` | Número inteiro | `42` |
| `double` | Número decimal | `3.14` |
| `boolean` | Verdadeiro ou falso | `true`, `false` |

### Declarando variáveis

```java
String nome = "Ana";
int idade = 25;
double altura = 1.68;
boolean estudante = true;

System.out.println(nome);     // Ana
System.out.println(idade);    // 25
System.out.println(altura);   // 1.68
System.out.println(estudante);// true
```

### Atribuindo novos valores

```java
int numero = 10;
System.out.println(numero); // 10

numero = 42;
System.out.println(numero); // 42
```

Note que ao reatribuir um valor, **não** repetimos o tipo.

### Boas práticas de nomeação

- Use **camelCase**: primeira palavra em minúsculo, demais começam em maiúsculo.
- Escolha nomes significativos.

```java
// Ruim
double a = 3.14;
double b = 22.0;
double c = a * b * b;

// Bom
double pi = 3.14;
double raio = 22.0;
double areaDoCirculo = pi * raio * raio;
```

### Lendo valores de outros tipos

```java
Scanner scanner = new Scanner(System.in);

System.out.print("Digite um número inteiro: ");
int numero = Integer.valueOf(scanner.nextLine());

System.out.print("Digite um número decimal: ");
double decimal = Double.valueOf(scanner.nextLine());

System.out.println("Inteiro: " + numero);
System.out.println("Decimal: " + decimal);
```

## Calculando com números

Java suporta as operações aritméticas básicas:

| Operador | Operação | Exemplo | Resultado |
|:--------:|:---------|:--------|:----------|
| `+` | Adição | `3 + 4` | `7` |
| `-` | Subtração | `10 - 3` | `7` |
| `*` | Multiplicação | `5 * 6` | `30` |
| `/` | Divisão | `10 / 3` | `3` *(inteira!)* |
| `%` | Resto | `10 % 3` | `1` |

### Divisão inteira vs. divisão decimal

Em Java, dividir dois `int` sempre dá resultado inteiro:

```java
System.out.println(10 / 3);    // 3  (parte decimal descartada)
System.out.println(10.0 / 3);  // 3.3333...
```

Para obter um resultado decimal a partir de inteiros, converta um deles:

```java
int a = 10;
int b = 3;
double resultado = (double) a / b;
System.out.println(resultado); // 3.3333...
```

### Exemplo completo

```java
import java.util.Scanner;

public class Calculadora {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Primeiro número: ");
        int a = Integer.valueOf(scanner.nextLine());

        System.out.print("Segundo número: ");
        int b = Integer.valueOf(scanner.nextLine());

        System.out.println("Soma: " + (a + b));
        System.out.println("Diferença: " + (a - b));
        System.out.println("Produto: " + (a * b));
        System.out.println("Quociente: " + (a / b));
        System.out.println("Resto: " + (a % b));
    }
}
```

## Condicionais

Condicionais permitem que o programa tome decisões.

### `if` e `else`

```java
int temperatura = 30;

if (temperatura > 25) {
    System.out.println("Está quente!");
} else {
    System.out.println("Não está tão quente.");
}
```

### `else if`

```java
int nota = 75;

if (nota >= 90) {
    System.out.println("A");
} else if (nota >= 80) {
    System.out.println("B");
} else if (nota >= 70) {
    System.out.println("C");
} else {
    System.out.println("Reprovado");
}
```

### Operadores de comparação

| Operador | Significado |
|:--------:|:------------|
| `==` | Igual a |
| `!=` | Diferente de |
| `<` | Menor que |
| `>` | Maior que |
| `<=` | Menor ou igual |
| `>=` | Maior ou igual |

### Operadores lógicos

| Operador | Significado | Exemplo |
|:--------:|:------------|:--------|
| `&&` | E (ambas verdadeiras) | `a > 0 && b > 0` |
| `\|\|` | Ou (ao menos uma) | `a > 0 \|\| b > 0` |
| `!` | Negação | `!ativo` |

### Comparando Strings

Para Strings, **não use `==`**. Use o método `.equals()`:

```java
String entrada = scanner.nextLine();

if (entrada.equals("sim")) {
    System.out.println("Você confirmou.");
} else {
    System.out.println("Você não confirmou.");
}
```

## Resumo

- Todo programa Java tem a estrutura `public class ... { public static void main ... }`.
- `System.out.println()` exibe texto e pula a linha; `System.out.print()` exibe sem pular.
- `Scanner` lê a entrada do usuário. Use `scanner.nextLine()` para ler texto.
- Os quatro tipos básicos são: `String`, `int`, `double` e `boolean`.
- Java realiza divisão inteira quando ambos os operandos são `int`.
- `if`, `else if` e `else` permitem que o programa tome decisões.
- Para comparar Strings, use `.equals()`, não `==`.

## Exercícios

**Exercício 1 — Olá, usuário!**

Escreva um programa que:
1. Pergunta o nome do usuário
2. Pergunta a idade do usuário
3. Exibe a mensagem: `Olá, [nome]! Você tem [idade] anos.`

**Exercício 2 — Calculadora simples**

Escreva um programa que lê dois números inteiros do usuário e exibe a soma, diferença, produto e quociente (com resultado decimal).

**Exercício 3 — Par ou ímpar**

Escreva um programa que lê um número inteiro e informa se é par ou ímpar.

**Exercício 4 — Classificação de temperatura**

Escreva um programa que lê uma temperatura em graus Celsius e exibe:
- `"Frio"` se for menor que 15
- `"Agradável"` se estiver entre 15 e 25 (inclusive)
- `"Quente"` se for maior que 25

**Exercício 5 — IMC**

Escreva um programa que lê o peso (kg) e a altura (m) do usuário, calcula o IMC (`peso / altura²`) e classifica:
- Menor que 18.5 → Abaixo do peso
- Entre 18.5 e 24.9 → Peso normal
- Entre 25 e 29.9 → Sobrepeso
- 30 ou mais → Obesidade
