---
title: "Programação em Java"
subtitle: "Lição 7: Paradigmas de Programação, Algoritmos e Exercícios Finais"
author: "Java Programming MOOC · Universidade de Helsinque — tradução para o português"
date: "2026"
lang: pt-BR
---

# Lição 7: Paradigmas de Programação, Algoritmos e Exercícios Finais

## Objetivos de aprendizagem

Ao final desta lição, você será capaz de:

- Compreender o conceito de paradigma de programação
- Distinguir a programação procedural da orientada a objetos
- Implementar o algoritmo de ordenação por seleção
- Usar `Arrays.sort()` e `Collections.sort()`
- Implementar busca linear e busca binária
- Distinguir métodos estáticos de métodos de instância
- Aplicar os conceitos do curso em exercícios de maior porte

---

## Parte 1: Paradigmas de Programação

### O que é um paradigma de programação?

Um **paradigma de programação** é uma maneira de pensar e estruturar a funcionalidade de um programa. Os paradigmas mais comuns são:

1. **Programação orientada a objetos** (POO)
2. **Programação procedural**
3. **Programação funcional**

### Programação orientada a objetos

Na POO, o programa é construído a partir de **objetos** que encapsulam estado e comportamento. Java foi projetada para este paradigma.

### Programação procedural

Na programação procedural, o programa é uma sequência de instruções e funções que processam dados recebidos como parâmetros. O estado vive em variáveis globais ou locais.

### Comparação: Relógio

**Versão procedural:**

```java
int horas = 0, minutos = 0, segundos = 0;

while (true) {
    System.out.printf("%02d:%02d:%02d%n", horas, minutos, segundos);
    segundos++;
    if (segundos >= 60) { segundos = 0; minutos++; }
    if (minutos >= 60)  { minutos = 0;  horas++;   }
    if (horas >= 24)    { horas = 0;               }
}
```

**Versão orientada a objetos:**

```java
public class Ponteiro {
    private int valor;
    private int limite;

    public Ponteiro(int limite) {
        this.limite = limite;
        this.valor = 0;
    }

    public void avanca() {
        this.valor++;
        if (this.valor >= this.limite) this.valor = 0;
    }

    public int getValor() { return this.valor; }

    @Override
    public String toString() {
        return (this.valor < 10 ? "0" : "") + this.valor;
    }
}

public class Relogio {
    private Ponteiro horas, minutos, segundos;

    public Relogio() {
        this.horas   = new Ponteiro(24);
        this.minutos = new Ponteiro(60);
        this.segundos = new Ponteiro(60);
    }

    public void avanca() {
        this.segundos.avanca();
        if (this.segundos.getValor() == 0) {
            this.minutos.avanca();
            if (this.minutos.getValor() == 0) {
                this.horas.avanca();
            }
        }
    }

    @Override
    public String toString() {
        return horas + ":" + minutos + ":" + segundos;
    }
}
```

| Aspecto | Procedural | Orientado a objetos |
|:--------|:-----------|:--------------------|
| Estado | Variáveis soltas | Encapsulado nos objetos |
| Responsabilidade | Concentrada | Distribuída |
| Reúso | Difícil | Fácil |

### Programação funcional em Java (Java 8+)

```java
List<Integer> numeros = List.of(1, 2, 3, 4, 5, 6);
int somaDosParES = numeros.stream()
    .filter(n -> n % 2 == 0)
    .mapToInt(Integer::intValue)
    .sum();
System.out.println(somaDosParES); // 12
```

---

## Parte 2: Algoritmos

### O que é um algoritmo?

Um **algoritmo** é uma sequência de instruções precisas para realizar uma tarefa. A **eficiência** de um algoritmo determina se a aplicação é viável na prática.

### Ordenação por seleção

```java
public static void ordenacaoPorSelecao(int[] vetor) {
    for (int i = 0; i < vetor.length - 1; i++) {
        int indiceMinimo = i;
        for (int j = i + 1; j < vetor.length; j++) {
            if (vetor[j] < vetor[indiceMinimo]) {
                indiceMinimo = j;
            }
        }
        int temp = vetor[i];
        vetor[i] = vetor[indiceMinimo];
        vetor[indiceMinimo] = temp;
    }
}
```

### Ordenação com métodos do Java

```java
import java.util.Arrays;
import java.util.Collections;

// Para arrays:
int[] numeros = {5, 2, 8, 1, 9};
Arrays.sort(numeros);
System.out.println(Arrays.toString(numeros)); // [1, 2, 5, 8, 9]

// Para listas:
ArrayList<Integer> lista = new ArrayList<>(Arrays.asList(5, 2, 8, 1));
Collections.sort(lista);
System.out.println(lista); // [1, 2, 5, 8]
```

### Busca linear

```java
public static int buscaLinear(int[] vetor, int alvo) {
    for (int i = 0; i < vetor.length; i++) {
        if (vetor[i] == alvo) return i;
    }
    return -1;
}
```

### Busca binária

```java
public static int buscaBinaria(int[] vetor, int alvo) {
    int esq = 0, dir = vetor.length - 1;
    while (esq <= dir) {
        int meio = (esq + dir) / 2;
        if (vetor[meio] == alvo)      return meio;
        else if (vetor[meio] < alvo)  esq = meio + 1;
        else                          dir = meio - 1;
    }
    return -1;
}
```

| Algoritmo | Dados desordenados? | Pior caso |
|:----------|:-------------------:|:---------:|
| Busca linear | Sim | O(*n*) |
| Busca binária | Não | O(log *n*) |

### Métodos estáticos vs. de instância

```java
// Método de instância — acessa o estado do objeto
public void incrementar() {
    this.valor++;
}

// Método estático — não depende de nenhum objeto
public static int maximo(int a, int b) {
    return a > b ? a : b;
}

// Chamada:
Matematica.maximo(5, 10); // sem criar objeto
```

---

## Parte 3: Exercícios de Programação Maiores

Esta parte coloca em prática todos os conceitos do Java Programming I. Não há estrutura predefinida — você decide as classes e relacionamentos.

**Dica:** identifique os substantivos (classes) e verbos (métodos) do enunciado antes de começar a codificar.

---

### Exercício 1 — Agenda de contatos

Implemente uma agenda de contatos com menu interativo.

**Classe `Contato`:** `nome`, `telefone`, `email`. Getters, `toString()`, `equals()` por nome.

**Classe `Agenda`:**
- `adicionarContato(Contato c)`
- `buscarPorNome(String nome)` — busca parcial, sem distinção de maiúsculas
- `removerContato(String nome)`
- `listarTodos()` — em ordem alfabética

**`main`:** menu com opções: adicionar, buscar, remover, listar, sair.

---

### Exercício 2 — Registro de notas

**Classe `Aluno`:** nome, matrícula, lista de notas. Métodos: `adicionarNota(int)` (0–10), `calcularMedia()`, `melhorNota()`, `piorNota()`, `aprovado(double notaMinima)`.

**Classe `Turma`:** lista de alunos. Métodos: `adicionarAluno`, `buscarPorMatricula`, `mediaGeral()`, `melhorAluno()`, `listarAprovados(double)`.

**`main`:** crie uma turma com ao menos 5 alunos com 3 notas cada. Exiba: média geral, melhor aluno, aprovados com nota ≥ 6.

---

### Exercício 3 — Loja simples

**Classe `Produto`:** nome, preço, estoque. Métodos: `vender(int qtd)`, `repor(int qtd)`.

**Classe `Carrinho`:** `HashMap<Produto, Integer>`. Métodos: `adicionar(Produto, int)`, `remover(Produto)`, `calcularTotal()`.

**Classe `Loja`:** lista de produtos. Métodos: `adicionarProduto`, `buscarProduto(String nome)`, `finalizarVenda(Carrinho)`.

**`main`:** simule um cliente adicionando 3 itens ao carrinho e finalizando a compra.

---

## Conclusão do Java Programming I

Você concluiu o **Java Programming I**! Ao longo das sete partes, aprendeu:

| Parte | Tópico |
|:-----:|:-------|
| 1 | Primeiros passos: variáveis, tipos, condicionais |
| 2 | Repetição com `while` e `for`, métodos |
| 3 | Listas (`ArrayList`), arrays, Strings |
| 4 | Orientação a objetos: classes, objetos, encapsulamento |
| 5 | OOP avançada: sobrecarga, referências, composição |
| 6 | Listas de objetos, separação UI/lógica, testes unitários |
| 7 | Paradigmas, algoritmos de ordenação e busca |

No **Java Programming II**, você aprenderá: herança, polimorfismo, interfaces, generics, streams, lambdas, exceções avançadas e muito mais.

**Parabéns e continue programando!**
