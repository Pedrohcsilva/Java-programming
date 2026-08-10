---
title: "Programação em Java"
subtitle: "Lição 3: Listas, Arrays e Strings"
author: "Java Programming MOOC · Universidade de Helsinque — tradução para o português"
date: "2026"
lang: pt-BR
---

# Lição 3: Listas, Arrays e Strings

## Objetivos de aprendizagem

Ao final desta lição, você será capaz de:

- Usar `ArrayList` para armazenar coleções de dados
- Adicionar, acessar, remover e buscar elementos em uma lista
- Percorrer listas com laços `for` e `for-each`
- Criar e usar arrays de tamanho fixo
- Trabalhar com métodos importantes da classe `String`
- Dividir strings com `split()`

## Descobrindo e corrigindo erros

Antes de falar sobre listas, é importante saber lidar com erros. Em Java, o compilador identifica erros de **sintaxe** (código mal escrito), mas erros de **lógica** só aparecem durante a execução.

### Tipos de erro

| Tipo | Quando ocorre | Exemplo |
|:-----|:-------------|:--------|
| Erro de sintaxe | Na compilação | Falta de `;` ou `}` |
| Erro de execução | Ao rodar o programa | Divisão por zero, `NullPointerException` |
| Erro de lógica | O programa roda, mas dá resultado errado | Usar `+` em vez de `-` |

### Stack trace

Quando um erro de execução ocorre, Java exibe um **stack trace** — uma lista das chamadas de método até o ponto do erro. Leia de baixo para cima para entender o que causou o problema:

```
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: Index 5 out of bounds for length 5
    at Programa.main(Programa.java:8)
```

Isso diz: o erro foi `ArrayIndexOutOfBoundsException` na linha 8 do arquivo `Programa.java`.

## Listas com `ArrayList`

Um `ArrayList` é uma coleção dinâmica que pode crescer ou diminuir conforme necessário. É muito mais prático do que declarar dezenas de variáveis separadas.

### Criando e adicionando elementos

```java
import java.util.ArrayList;

ArrayList<String> nomes = new ArrayList<>();
nomes.add("Alice");
nomes.add("Bruno");
nomes.add("Carlos");

System.out.println(nomes); // [Alice, Bruno, Carlos]
```

> Para tipos primitivos como `int` e `double`, use os equivalentes em maiúsculo: `Integer`, `Double`.

```java
ArrayList<Integer> numeros = new ArrayList<>();
numeros.add(10);
numeros.add(20);
numeros.add(30);
```

### Acessando elementos

Os índices começam em 0:

```java
System.out.println(nomes.get(0)); // Alice
System.out.println(nomes.get(1)); // Bruno
System.out.println(nomes.size()); // 3
```

### Removendo elementos

```java
nomes.remove("Bruno");        // remove pelo valor
System.out.println(nomes);    // [Alice, Carlos]

nomes.remove(0);              // remove pelo índice
System.out.println(nomes);    // [Carlos]
```

### Verificando existência

```java
ArrayList<String> frutas = new ArrayList<>();
frutas.add("maçã");
frutas.add("banana");

System.out.println(frutas.contains("banana")); // true
System.out.println(frutas.contains("uva"));    // false
```

### Percorrendo listas

**Com `for` indexado:**

```java
for (int i = 0; i < nomes.size(); i++) {
    System.out.println(i + ": " + nomes.get(i));
}
```

**Com `for-each` (mais legível):**

```java
for (String nome : nomes) {
    System.out.println(nome);
}
```

**Com `while`:**

```java
int i = 0;
while (i < nomes.size()) {
    System.out.println(nomes.get(i));
    i++;
}
```

### Lista como parâmetro de método

Como `ArrayList` é um tipo de referência, o método recebe a referência ao objeto original — alterações dentro do método afetam a lista original:

```java
public static void adicionarZero(ArrayList<Integer> lista) {
    lista.add(0);
}

public static void main(String[] args) {
    ArrayList<Integer> numeros = new ArrayList<>();
    numeros.add(1);
    numeros.add(2);

    adicionarZero(numeros);
    System.out.println(numeros); // [1, 2, 0]
}
```

### Exemplo: soma e média de uma lista

```java
public static double calcularMedia(ArrayList<Integer> lista) {
    if (lista.size() == 0) {
        return 0.0;
    }
    int soma = 0;
    for (int n : lista) {
        soma += n;
    }
    return (double) soma / lista.size();
}
```

## Arrays

Um **array** é uma coleção de **tamanho fixo** do mesmo tipo. Diferentemente do `ArrayList`, o tamanho não pode ser alterado após a criação.

### Criando arrays

```java
int[] numeros = new int[5];         // array de 5 inteiros, todos 0
String[] nomes = new String[3];     // array de 3 strings, todas null

// Inicialização direta
int[] valores = {10, 20, 30, 40, 50};
```

### Acessando e modificando elementos

```java
int[] arr = {5, 10, 15, 20};

System.out.println(arr[0]); // 5
System.out.println(arr[3]); // 20

arr[1] = 99;
System.out.println(arr[1]); // 99
```

### Tamanho do array

```java
int[] arr = {1, 2, 3, 4, 5};
System.out.println(arr.length); // 5
```

### Percorrendo um array

```java
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}

// Ou com for-each:
for (int n : arr) {
    System.out.println(n);
}
```

### Quando usar array vs. ArrayList?

| Critério | Array | ArrayList |
|:---------|:-----:|:---------:|
| Tamanho fixo | Sim | Não |
| Mais eficiente | Sim | Não |
| Mais flexível | Não | Sim |
| Tipos primitivos diretos | Sim | Não (usa wrapper) |

Use arrays quando o tamanho for fixo e conhecido. Prefira `ArrayList` para a maioria dos casos.

## Trabalhando com Strings

`String` é uma das classes mais usadas em Java. Ela possui muitos métodos úteis.

### Métodos principais

```java
String texto = "Olá, mundo!";

System.out.println(texto.length());        // 11 — tamanho
System.out.println(texto.toUpperCase());   // OLÁ, MUNDO!
System.out.println(texto.toLowerCase());   // olá, mundo!
System.out.println(texto.contains("mundo")); // true
System.out.println(texto.startsWith("Olá")); // true
System.out.println(texto.endsWith("!"));     // true
System.out.println(texto.indexOf("mundo"));  // 5 — posição
System.out.println(texto.replace("mundo", "Java")); // Olá, Java!
System.out.println(texto.trim());          // remove espaços das bordas
```

### Acessando caracteres

```java
String palavra = "Java";
char c = palavra.charAt(0); // J
System.out.println(c);
```

### Comparando strings

**Sempre use `.equals()`, nunca `==`:**

```java
String a = "Java";
String b = "Java";

System.out.println(a.equals(b));               // true
System.out.println(a.equalsIgnoreCase("java")); // true — ignora maiúsculas/minúsculas
```

### Dividindo strings com `split()`

O método `split()` divide uma string em partes, retornando um array de Strings:

```java
String linha = "Alice,25,São Paulo";
String[] partes = linha.split(",");

System.out.println(partes[0]); // Alice
System.out.println(partes[1]); // 25
System.out.println(partes[2]); // São Paulo
```

```java
String frase = "Java é uma linguagem poderosa";
String[] palavras = frase.split(" ");

for (String palavra : palavras) {
    System.out.println(palavra);
}
```

### Convertendo tipos

```java
int numero = Integer.valueOf("42");
double decimal = Double.valueOf("3.14");
String texto = String.valueOf(100); // "100"
```

### Exemplo: processando dados CSV

```java
import java.util.Scanner;

public class ProcessadorCSV {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.println("Digite registros no formato 'nome,idade' (ou 'fim' para encerrar):");

        ArrayList<String> nomes = new ArrayList<>();
        ArrayList<Integer> idades = new ArrayList<>();

        while (true) {
            String linha = scanner.nextLine();
            if (linha.equals("fim")) {
                break;
            }
            String[] partes = linha.split(",");
            nomes.add(partes[0]);
            idades.add(Integer.valueOf(partes[1]));
        }

        System.out.println("\n--- Registros ---");
        for (int i = 0; i < nomes.size(); i++) {
            System.out.println(nomes.get(i) + " tem " + idades.get(i) + " anos.");
        }
    }
}
```

## Resumo

- `ArrayList` é uma coleção dinâmica; use `add()`, `get()`, `remove()`, `contains()`, `size()`.
- Índices começam em 0; acessar índice inválido lança `IndexOutOfBoundsException`.
- Arrays têm tamanho fixo, declarado com `new tipo[tamanho]` ou literal `{v1, v2, ...}`.
- `String` tem métodos como `length()`, `contains()`, `split()`, `equals()`, `charAt()`.
- Para comparar strings, use `.equals()` — nunca `==`.
- `split(",")` divide uma string em partes usando o delimitador fornecido.

## Exercícios

**Exercício 1 — Lista de compras**

Escreva um programa que:
- Lê itens de uma lista de compras até o usuário digitar `"sair"`
- Exibe todos os itens numerados
- Informa a quantidade total de itens

**Exercício 2 — Mínimo e máximo**

Crie um método `minimo(ArrayList<Integer> lista)` e `maximo(ArrayList<Integer> lista)`. Popule uma lista com 6 números lidos do usuário e exiba o menor e o maior.

**Exercício 3 — Palavras únicas**

Leia palavras do usuário (até "fim") e armazene apenas as que ainda não foram adicionadas à lista (sem duplicatas). Ao final, exiba todas as palavras únicas.

**Exercício 4 — Invertendo um array**

Crie um método `inverter(int[] arr)` que retorna um novo array com os elementos na ordem inversa. Não modifique o array original.

**Exercício 5 — Análise de frase**

Escreva um programa que lê uma frase e exibe:
- Número de palavras
- A palavra mais longa
- A palavra mais curta
- A frase com todas as palavras em maiúsculo
