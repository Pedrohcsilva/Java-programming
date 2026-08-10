---
title: "Programação em Java"
subtitle: "Lição 5: Orientação a Objetos — Aprofundamento"
author: "Java Programming MOOC · Universidade de Helsinque — tradução para o português"
date: "2026"
lang: pt-BR
---

# Lição 5: Orientação a Objetos — Aprofundamento

## Objetivos de aprendizagem

Ao final desta lição, você será capaz de:

- Criar múltiplos construtores e métodos sobrecarregados
- Entender a diferença entre variáveis primitivas e de referência
- Comparar objetos com o método `equals()`
- Usar objetos como variáveis de instância (composição)
- Compreender o que é `null` e como evitar `NullPointerException`

## Sobrecarga de construtores

Uma classe pode ter **vários construtores**, desde que difiram no número ou tipo de parâmetros. Isso se chama **sobrecarga** (*overloading*).

```java
public class Pessoa {
    private String nome;
    private int idade;

    // Construtor com apenas o nome
    public Pessoa(String nome) {
        this.nome = nome;
        this.idade = 0;
    }

    // Construtor com nome e idade
    public Pessoa(String nome, int idade) {
        this.nome = nome;
        this.idade = idade;
    }
}
```

Uso:

```java
Pessoa alice = new Pessoa("Alice");       // idade = 0
Pessoa bruno = new Pessoa("Bruno", 30);  // idade = 30
```

### Evitando repetição com `this()`

Um construtor pode chamar outro usando `this()`. Essa chamada deve ser a **primeira instrução**:

```java
public Pessoa(String nome) {
    this(nome, 0); // chama o construtor de dois parâmetros
}

public Pessoa(String nome, int idade) {
    this.nome = nome;
    this.idade = idade;
}
```

## Sobrecarga de métodos

Da mesma forma, podemos ter **múltiplos métodos com o mesmo nome**, diferindo apenas nos parâmetros:

```java
public void crescer() {
    this.idade++;
}

public void crescer(int anos) {
    this.idade += anos;
}
```

Uso:

```java
alice.crescer();     // envelhece 1 ano
alice.crescer(5);    // envelhece 5 anos
```

Versão sem repetição (usando `this.metodo()`):

```java
public void crescer() {
    this.crescer(1);
}

public void crescer(int anos) {
    this.idade += anos;
}
```

## Variáveis primitivas vs. variáveis de referência

### Primitivas

Os tipos `int`, `double`, `boolean`, `char` etc. armazenam diretamente o valor na memória:

```java
int a = 5;
int b = a; // b recebe uma CÓPIA do valor 5

b = 10;
System.out.println(a); // 5 — a não mudou
System.out.println(b); // 10
```

### Referências

Objetos (como `String`, `ArrayList`, ou qualquer classe criada) armazenam uma **referência** ao objeto na memória:

```java
Pessoa p1 = new Pessoa("Ana");
Pessoa p2 = p1; // p2 aponta para o MESMO objeto que p1

p2.crescer();
System.out.println(p1.getIdade()); // 1 — p1 também mudou!
```

Para ter **dois objetos independentes**, crie cada um separadamente:

```java
Pessoa p1 = new Pessoa("Ana");
Pessoa p2 = new Pessoa("Ana"); // objeto diferente
```

### O valor `null`

Uma variável de referência que não aponta para nenhum objeto tem o valor `null`:

```java
Pessoa p = null;
System.out.println(p); // null

p.getNome(); // ERRO: NullPointerException!
```

Sempre verifique se uma referência é `null` antes de usá-la:

```java
if (p != null) {
    System.out.println(p.getNome());
}
```

## Comparando objetos com `equals()`

O operador `==` compara **referências** (se apontam para o mesmo objeto), não os **conteúdos**:

```java
Pessoa p1 = new Pessoa("Ana", 25);
Pessoa p2 = new Pessoa("Ana", 25);

System.out.println(p1 == p2); // false — são objetos diferentes na memória
```

Para comparar pelo **conteúdo**, implemente o método `equals()`:

```java
@Override
public boolean equals(Object obj) {
    if (this == obj) return true;           // mesmo objeto
    if (!(obj instanceof Pessoa)) return false; // tipo diferente

    Pessoa outra = (Pessoa) obj;
    return this.nome.equals(outra.nome) && this.idade == outra.idade;
}
```

```java
System.out.println(p1.equals(p2)); // true — mesmos atributos
```

## Objetos como variáveis de instância (composição)

Um objeto pode **conter** outros objetos como variáveis de instância. Isso se chama **composição**:

```java
public class Endereco {
    private String rua;
    private String cidade;
    private String cep;

    public Endereco(String rua, String cidade, String cep) {
        this.rua = rua;
        this.cidade = cidade;
        this.cep = cep;
    }

    @Override
    public String toString() {
        return this.rua + ", " + this.cidade + " - " + this.cep;
    }
}
```

```java
public class Pessoa {
    private String nome;
    private Endereco endereco; // composição!

    public Pessoa(String nome, Endereco endereco) {
        this.nome = nome;
        this.endereco = endereco;
    }

    @Override
    public String toString() {
        return this.nome + "\n  " + this.endereco;
    }
}
```

Uso:

```java
Endereco end = new Endereco("Rua das Flores, 100", "São Paulo", "01310-100");
Pessoa p = new Pessoa("Maria", end);
System.out.println(p);
```

**Saída:**
```
Maria
  Rua das Flores, 100, São Paulo - 01310-100
```

## Exemplo completo: Datas simples

```java
public class Data {
    private int dia;
    private int mes;
    private int ano;

    public Data(int dia, int mes, int ano) {
        this.dia = dia;
        this.mes = mes;
        this.ano = ano;
    }

    public boolean ehAnterior(Data outra) {
        if (this.ano != outra.ano) return this.ano < outra.ano;
        if (this.mes != outra.mes) return this.mes < outra.mes;
        return this.dia < outra.dia;
    }

    @Override
    public boolean equals(Object obj) {
        if (!(obj instanceof Data)) return false;
        Data outra = (Data) obj;
        return this.dia == outra.dia && this.mes == outra.mes && this.ano == outra.ano;
    }

    @Override
    public String toString() {
        return String.format("%02d/%02d/%04d", this.dia, this.mes, this.ano);
    }
}
```

```java
Data natal = new Data(25, 12, 2026);
Data anoNovo = new Data(1, 1, 2027);

System.out.println(natal.ehAnterior(anoNovo)); // true
System.out.println(natal.equals(anoNovo));      // false
System.out.println(natal);                      // 25/12/2026
```

## Continuando o aprendizado de OOP

À medida que os programas crescem, a OOP torna-se ainda mais valiosa. Na próxima parte, veremos como separar a interface do usuário da lógica do programa e como escrever testes.

### Dicas práticas

- **Responsabilidade única**: cada classe deve ter um único propósito bem definido.
- **Evite objetos gigantes**: se uma classe ficou muito grande, considere dividi-la.
- **Prefira composição**: em vez de tentar resolver tudo numa classe só, componha classes menores.

## Resumo

- **Sobrecarga**: múltiplos construtores/métodos com o mesmo nome mas parâmetros diferentes.
- `this()` chama outro construtor da mesma classe (deve ser a primeira linha).
- **Primitivas** copiam valores; **referências** copiam ponteiros para objetos.
- `null` representa ausência de objeto; acessar métodos em `null` causa `NullPointerException`.
- `==` compara referências; `equals()` compara conteúdo (implemente-o para suas classes).
- **Composição**: um objeto pode conter outros objetos como atributos.

## Exercícios

**Exercício 1 — Sobrecarga de construtores**

Crie a classe `Livro` com os atributos `titulo`, `autor` e `anoPublicacao`. Implemente:
- Construtor com os três atributos
- Construtor apenas com `titulo` e `autor` (ano = 0)
- Construtor apenas com `titulo` (autor = "Desconhecido", ano = 0)
- `toString()` e `equals()` (igualdade por título e autor)

**Exercício 2 — Referências vs. primitivas**

Escreva um programa que demonstre a diferença entre copiar um `int` e copiar uma referência a um objeto. Mostre que alterar a cópia de um `int` não afeta o original, mas alterar via uma referência copiada afeta o objeto original.

**Exercício 3 — Ponto no plano**

Crie a classe `Ponto` com `x` e `y` (double). Implemente:
- `distanciaAte(Ponto outro)` usando `Math.sqrt`
- `equals(Object obj)` com precisão de 0.001
- `toString()` no formato `(x, y)`

Crie a classe `Circulo` com um `Ponto` centro e um raio `double`. Implemente:
- `contemPonto(Ponto p)` — verifica se o ponto está dentro do círculo
- `toString()`

**Exercício 4 — Null safety**

Crie um método `imprimirPessoa(Pessoa p)` que:
- Se `p` for `null`, exibe `"Pessoa não encontrada."`
- Caso contrário, exibe as informações da pessoa

Teste chamando o método com `null` e com um objeto válido.
