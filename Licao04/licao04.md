---
title: "Programação em Java"
subtitle: "Lição 4: Orientação a Objetos — Fundamentos"
author: "Java Programming MOOC · Universidade de Helsinque — tradução para o português"
date: "2026"
lang: pt-BR
---

# Lição 4: Orientação a Objetos — Fundamentos

## Objetivos de aprendizagem

Ao final desta lição, você será capaz de:

- Entender o que são classes e objetos
- Criar classes com variáveis de instância, construtores e métodos
- Usar `this` para referenciar o objeto atual
- Implementar encapsulamento com `private` e métodos getters/setters
- Armazenar objetos em listas (`ArrayList`)
- Ler dados de arquivos e representá-los como objetos

## O que são classes e objetos?

Uma **classe** é um molde (blueprint) que descreve um conceito do mundo real: seus atributos e comportamentos. Um **objeto** é uma instância concreta desse molde.

Analogia: a **planta baixa** de uma casa é a classe. Cada **casa construída** a partir dessa planta é um objeto. Todas seguem o mesmo projeto, mas cada uma tem características próprias (cor, moradores, etc.).

### Por que usar orientação a objetos?

- Modela o problema de forma natural (conceitos do domínio viram classes)
- Facilita a organização e manutenção de programas grandes
- Promove o reúso de código

## Criando uma classe

```java
public class Pessoa {
    // variáveis de instância (atributos)
    private String nome;
    private int idade;

    // construtor
    public Pessoa(String nome) {
        this.nome = nome;
        this.idade = 0;
    }

    // métodos
    public void crescer() {
        this.idade++;
    }

    public String getNome() {
        return this.nome;
    }

    public int getIdade() {
        return this.idade;
    }

    @Override
    public String toString() {
        return this.nome + ", " + this.idade + " anos";
    }
}
```

### Usando a classe

```java
Pessoa alice = new Pessoa("Alice");
Pessoa bruno = new Pessoa("Bruno");

alice.crescer();
alice.crescer();

System.out.println(alice);       // Alice, 2 anos
System.out.println(bruno);       // Bruno, 0 anos
System.out.println(alice.getNome()); // Alice
```

Cada objeto tem seu **próprio** conjunto de variáveis de instância — alterar `alice` não afeta `bruno`.

## Variáveis de instância

As variáveis de instância guardam o **estado** do objeto. Em geral, são declaradas como `private` para protegê-las de acesso externo direto.

```java
public class ContaBancaria {
    private String titular;
    private double saldo;

    public ContaBancaria(String titular, double saldoInicial) {
        this.titular = titular;
        this.saldo = saldoInicial;
    }
}
```

## Construtores

O **construtor** é chamado com `new` e inicializa as variáveis de instância. Tem o mesmo nome da classe e não declara tipo de retorno.

```java
ContaBancaria conta = new ContaBancaria("Maria", 1000.0);
```

Se você não definir um construtor, Java fornece um padrão (sem parâmetros), mas assim que você define um, o padrão deixa de existir.

## O `this`

A palavra `this` dentro de um método refere-se ao **objeto que chamou o método**:

```java
public class Ponto {
    private int x;
    private int y;

    public Ponto(int x, int y) {
        this.x = x; // this.x é a variável de instância; x é o parâmetro
        this.y = y;
    }

    public double distanciaOrigem() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }
}
```

## Encapsulamento: `private` e getters/setters

**Encapsulamento** significa esconder os detalhes internos do objeto. Declaramos variáveis como `private` e fornecemos métodos públicos para lê-las ou alterá-las.

### Getters (leitura)

```java
public String getNome() {
    return this.nome;
}

public int getIdade() {
    return this.idade;
}

public boolean isMaiorDeIdade() {
    return this.idade >= 18;
}
```

### Setters (escrita com validação)

```java
public void setIdade(int idade) {
    if (idade >= 0) {
        this.idade = idade;
    }
}

public void setSaldo(double saldo) {
    if (saldo >= 0) {
        this.saldo = saldo;
    }
}
```

O setter pode **validar** o valor antes de atribuí-lo — isso é impossível com acesso direto à variável.

## O método `toString()`

O método `toString()` define como o objeto é representado como texto. É chamado automaticamente quando o objeto é passado para `System.out.println()` ou concatenado com uma string.

```java
@Override
public String toString() {
    return this.titular + " | Saldo: R$ " + this.saldo;
}
```

```java
ContaBancaria conta = new ContaBancaria("João", 500.0);
System.out.println(conta); // João | Saldo: R$ 500.0
```

## Métodos que modificam o estado

```java
public class ContaBancaria {
    private String titular;
    private double saldo;

    public ContaBancaria(String titular, double saldoInicial) {
        this.titular = titular;
        this.saldo = saldoInicial;
    }

    public void depositar(double valor) {
        if (valor > 0) {
            this.saldo += valor;
        }
    }

    public boolean sacar(double valor) {
        if (valor > 0 && this.saldo >= valor) {
            this.saldo -= valor;
            return true;
        }
        return false;
    }

    public double getSaldo() {
        return this.saldo;
    }

    @Override
    public String toString() {
        return this.titular + " | Saldo: R$ " + String.format("%.2f", this.saldo);
    }
}
```

```java
ContaBancaria conta = new ContaBancaria("Carlos", 200.0);
conta.depositar(300.0);
conta.sacar(100.0);
System.out.println(conta); // Carlos | Saldo: R$ 400,00
```

## Objetos em listas

Objetos podem ser armazenados em `ArrayList` como qualquer outro tipo:

```java
ArrayList<Pessoa> pessoas = new ArrayList<>();
pessoas.add(new Pessoa("Ana"));
pessoas.add(new Pessoa("Bob"));
pessoas.add(new Pessoa("Carol"));

for (Pessoa p : pessoas) {
    System.out.println(p);
}
```

### Buscando um objeto na lista

```java
public static Pessoa buscarPorNome(ArrayList<Pessoa> lista, String nome) {
    for (Pessoa p : lista) {
        if (p.getNome().equals(nome)) {
            return p;
        }
    }
    return null; // não encontrado
}
```

## Lendo dados de arquivos

Java pode ler arquivos de texto com `Scanner` e `File`:

```java
import java.util.Scanner;
import java.io.File;
import java.io.FileNotFoundException;

public class LeitorDeArquivo {
    public static void main(String[] args) throws FileNotFoundException {
        Scanner scanner = new Scanner(new File("pessoas.txt"));

        ArrayList<Pessoa> pessoas = new ArrayList<>();

        while (scanner.hasNextLine()) {
            String linha = scanner.nextLine();
            String[] partes = linha.split(",");
            String nome = partes[0];
            int idade = Integer.valueOf(partes[1]);

            Pessoa p = new Pessoa(nome);
            // se Pessoa tivesse setIdade:
            // p.setIdade(idade);

            pessoas.add(p);
        }

        for (Pessoa p : pessoas) {
            System.out.println(p);
        }
    }
}
```

Exemplo de arquivo `pessoas.txt`:
```
Alice,30
Bruno,25
Carol,28
```

## Exemplo completo: classe `Produto`

```java
public class Produto {
    private String nome;
    private double preco;
    private int estoque;

    public Produto(String nome, double preco, int estoque) {
        this.nome = nome;
        this.preco = preco;
        this.estoque = estoque;
    }

    public String getNome() { return this.nome; }
    public double getPreco() { return this.preco; }
    public int getEstoque() { return this.estoque; }

    public boolean vender(int quantidade) {
        if (quantidade > this.estoque) {
            System.out.println("Estoque insuficiente!");
            return false;
        }
        this.estoque -= quantidade;
        return true;
    }

    public void repor(int quantidade) {
        this.estoque += quantidade;
    }

    @Override
    public String toString() {
        return this.nome + " | R$ " + String.format("%.2f", this.preco)
               + " | Estoque: " + this.estoque;
    }
}
```

## Resumo

- Uma **classe** define o molde; um **objeto** é uma instância da classe.
- **Variáveis de instância** (`private`) guardam o estado do objeto.
- O **construtor** inicializa o objeto ao criá-lo com `new`.
- **`this`** refere-se ao próprio objeto dentro dos seus métodos.
- **Encapsulamento**: atributos `private`, acesso via getters e setters.
- **`toString()`** define a representação textual do objeto.
- Objetos podem ser armazenados em `ArrayList<NomeDaClasse>`.

## Exercícios

**Exercício 1 — Retângulo**

Crie a classe `Retangulo` com atributos `largura` e `altura` (ambos `double`, `private`). Adicione:
- Construtor com os dois valores
- Métodos `getArea()` e `getPerimetro()`
- Método `ehQuadrado()` que retorna `boolean`
- `toString()` descritivo

**Exercício 2 — Aluno e notas**

Crie a classe `Aluno` com `nome` (String) e uma lista de notas (`ArrayList<Integer>`). Adicione:
- Método `adicionarNota(int nota)` (valide: nota entre 0 e 10)
- Método `calcularMedia()` retornando `double`
- Método `aprovado()` que retorna `true` se média >= 6
- `toString()` mostrando nome, média e situação

**Exercício 3 — Estacionamento**

Crie a classe `Veiculo` com `placa` (String) e `marca` (String). Crie a classe `Estacionamento` com:
- Capacidade máxima (definida no construtor)
- Lista de veículos
- Métodos `entrar(Veiculo v)` (verifica se há vaga), `sair(String placa)`, `listarVeiculos()`

**Exercício 4 — Leitura de arquivo**

Crie um arquivo `produtos.txt` com linhas no formato `nome,preco,estoque`. Leia o arquivo, crie objetos `Produto` e exiba todos os produtos. Calcule o valor total em estoque (preço × quantidade).
