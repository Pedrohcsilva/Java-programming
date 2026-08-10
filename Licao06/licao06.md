---
title: "Programação em Java"
subtitle: "Lição 6: Listas de Objetos, Testes e Programas Complexos"
author: "Java Programming MOOC · Universidade de Helsinque — tradução para o português"
date: "2026"
lang: pt-BR
---

# Lição 6: Listas de Objetos, Testes e Programas Complexos

## Objetivos de aprendizagem

Ao final desta lição, você será capaz de:

- Usar listas de objetos e objetos que contêm listas
- Separar a interface do usuário da lógica do programa
- Escrever e executar testes unitários com JUnit
- Organizar programas maiores em múltiplas classes
- Entender o conceito de desenvolvimento orientado a testes (TDD)

## Objetos em listas e listas dentro de objetos

### Lista de objetos

Já vimos que podemos armazenar objetos em listas. Vejamos um exemplo mais completo:

```java
ArrayList<Livro> biblioteca = new ArrayList<>();
biblioteca.add(new Livro("Dom Casmurro", "Machado de Assis", 1899));
biblioteca.add(new Livro("O Cortiço", "Aluísio Azevedo", 1890));
biblioteca.add(new Livro("Vidas Secas", "Graciliano Ramos", 1938));
```

Buscando um livro por título:

```java
public static Livro buscarPorTitulo(ArrayList<Livro> lista, String titulo) {
    for (Livro livro : lista) {
        if (livro.getTitulo().equalsIgnoreCase(titulo)) {
            return livro;
        }
    }
    return null;
}
```

Filtrando por ano:

```java
public static ArrayList<Livro> livrosAntesDe(ArrayList<Livro> lista, int ano) {
    ArrayList<Livro> resultado = new ArrayList<>();
    for (Livro livro : lista) {
        if (livro.getAno() < ano) {
            resultado.add(livro);
        }
    }
    return resultado;
}
```

### Objeto com lista interna

Uma classe pode ter um `ArrayList` como variável de instância:

```java
public class Turma {
    private String nome;
    private ArrayList<Aluno> alunos;

    public Turma(String nome) {
        this.nome = nome;
        this.alunos = new ArrayList<>();
    }

    public void adicionarAluno(Aluno aluno) {
        this.alunos.add(aluno);
    }

    public int quantidadeDeAlunos() {
        return this.alunos.size();
    }

    public double mediaGeral() {
        if (this.alunos.isEmpty()) return 0.0;

        double somaMedias = 0;
        for (Aluno aluno : this.alunos) {
            somaMedias += aluno.calcularMedia();
        }
        return somaMedias / this.alunos.size();
    }

    public void listarAprovados(double notaMinima) {
        System.out.println("Aprovados em " + this.nome + ":");
        for (Aluno aluno : this.alunos) {
            if (aluno.calcularMedia() >= notaMinima) {
                System.out.println("  " + aluno);
            }
        }
    }

    @Override
    public String toString() {
        return this.nome + " (" + this.alunos.size() + " alunos)";
    }
}
```

## Separando a interface do usuário da lógica do programa

Um princípio fundamental de bom design é **separar** o que o programa faz (lógica) de como ele interage com o usuário (interface).

### O problema de misturar tudo

```java
// Ruim: lógica e UI juntas
public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);
    ArrayList<String> itens = new ArrayList<>();

    System.out.print("Quantos itens? ");
    int n = Integer.valueOf(scanner.nextLine());

    for (int i = 0; i < n; i++) {
        System.out.print("Item " + (i+1) + ": ");
        itens.add(scanner.nextLine());
    }

    // lógica misturada com UI...
}
```

### A solução: classes separadas

**Classe de lógica:**

```java
public class ListaDeCompras {
    private ArrayList<String> itens;

    public ListaDeCompras() {
        this.itens = new ArrayList<>();
    }

    public void adicionar(String item) {
        this.itens.add(item);
    }

    public boolean remover(String item) {
        return this.itens.remove(item);
    }

    public boolean contem(String item) {
        return this.itens.contains(item);
    }

    public int tamanho() {
        return this.itens.size();
    }

    public ArrayList<String> getItens() {
        return new ArrayList<>(this.itens); // retorna cópia, não referência direta
    }

    @Override
    public String toString() {
        return this.itens.toString();
    }
}
```

**Classe de interface:**

```java
public class InterfaceListaDeCompras {
    private Scanner scanner;
    private ListaDeCompras lista;

    public InterfaceListaDeCompras(Scanner scanner) {
        this.scanner = scanner;
        this.lista = new ListaDeCompras();
    }

    public void executar() {
        while (true) {
            System.out.println("\n1. Adicionar  2. Remover  3. Listar  4. Sair");
            System.out.print("Escolha: ");
            String opcao = scanner.nextLine();

            if (opcao.equals("1")) {
                adicionarItem();
            } else if (opcao.equals("2")) {
                removerItem();
            } else if (opcao.equals("3")) {
                listarItens();
            } else if (opcao.equals("4")) {
                System.out.println("Até logo!");
                break;
            } else {
                System.out.println("Opção inválida.");
            }
        }
    }

    private void adicionarItem() {
        System.out.print("Item: ");
        String item = scanner.nextLine();
        this.lista.adicionar(item);
        System.out.println("'" + item + "' adicionado.");
    }

    private void removerItem() {
        System.out.print("Item a remover: ");
        String item = scanner.nextLine();
        if (this.lista.remover(item)) {
            System.out.println("Removido.");
        } else {
            System.out.println("Item não encontrado.");
        }
    }

    private void listarItens() {
        System.out.println("Sua lista (" + this.lista.tamanho() + " itens):");
        for (String item : this.lista.getItens()) {
            System.out.println("  - " + item);
        }
    }
}
```

**`main` simples:**

```java
public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);
    InterfaceListaDeCompras ui = new InterfaceListaDeCompras(scanner);
    ui.executar();
}
```

## Introdução a testes unitários

**Testes unitários** verificam automaticamente se um método ou classe se comporta corretamente. Em Java, usamos a biblioteca **JUnit**.

### Por que testar?

- Detecta bugs antes de chegar ao usuário
- Dá confiança para refatorar o código
- Serve como documentação do comportamento esperado

### Estrutura de um teste JUnit

```java
import org.junit.Test;
import static org.junit.Assert.*;

public class CalculadoraTest {

    @Test
    public void testSoma() {
        Calculadora calc = new Calculadora();
        assertEquals(5, calc.soma(2, 3));
    }

    @Test
    public void testSubtracao() {
        Calculadora calc = new Calculadora();
        assertEquals(1, calc.subtracao(3, 2));
    }

    @Test
    public void testDivisaoPorZero() {
        Calculadora calc = new Calculadora();
        assertEquals(0, calc.divisao(10, 0));
    }
}
```

Classe sendo testada:

```java
public class Calculadora {
    private int valor;

    public Calculadora() {
        this.valor = 0;
    }

    public int soma(int a, int b) {
        return a + b;
    }

    public int subtracao(int a, int b) {
        return a - b;
    }

    public int divisao(int a, int b) {
        if (b == 0) return 0;
        return a / b;
    }
}
```

### Métodos de asserção do JUnit

| Método | Verifica |
|:-------|:---------|
| `assertEquals(esperado, real)` | Se dois valores são iguais |
| `assertNotEquals(naoEsperado, real)` | Se dois valores são diferentes |
| `assertTrue(condicao)` | Se a condição é verdadeira |
| `assertFalse(condicao)` | Se a condição é falsa |
| `assertNull(objeto)` | Se o objeto é `null` |
| `assertNotNull(objeto)` | Se o objeto não é `null` |

### Desenvolvimento orientado a testes (TDD)

O **TDD** (*Test-Driven Development*) inverte o processo tradicional:

1. **Escreva o teste** para o comportamento desejado (antes de implementar)
2. **Execute** — o teste falha (código ainda não existe)
3. **Implemente** o mínimo de código para o teste passar
4. **Execute** — o teste passa
5. **Refatore** o código mantendo os testes passando

```java
// Passo 1: escreva o teste primeiro
@Test
public void testListaVaziaRetornaZeroItens() {
    ListaDeCompras lista = new ListaDeCompras();
    assertEquals(0, lista.tamanho());
}

// Passo 1: teste para adicionar item
@Test
public void testAdicionarItemAumentaTamanho() {
    ListaDeCompras lista = new ListaDeCompras();
    lista.adicionar("pão");
    assertEquals(1, lista.tamanho());
}

// Passo 1: teste para remover item
@Test
public void testRemoverItemDiminuiTamanho() {
    ListaDeCompras lista = new ListaDeCompras();
    lista.adicionar("leite");
    lista.remover("leite");
    assertEquals(0, lista.tamanho());
}
```

## Lendo e interpretando o stack trace

Quando um erro ocorre, o Java exibe uma trilha de chamadas. Aprenda a ler:

```
Exception in thread "main" java.lang.NullPointerException
    at Turma.mediaGeral(Turma.java:23)
    at Main.main(Main.java:10)
```

- **Tipo do erro**: `NullPointerException` — tentativa de usar uma referência `null`
- **Onde**: linha 23 do arquivo `Turma.java`, dentro do método `mediaGeral`
- **Chamado de**: linha 10 do `Main.java`

Leia de baixo para cima: o `main` chamou `mediaGeral`, que falhou na linha 23.

## Resumo

- Listas podem conter objetos; classes podem ter listas como variáveis de instância.
- **Separe a lógica da interface do usuário** — lógica em classes próprias, UI em outra classe.
- **Testes unitários** verificam o comportamento de métodos isoladamente.
- Use `assertEquals`, `assertTrue`, `assertFalse` etc. para verificar resultados.
- **TDD**: escreva o teste antes do código — isso guia um design melhor.
- O **stack trace** aponta a linha exata onde o erro ocorreu.

## Exercícios

**Exercício 1 — Biblioteca**

Crie a classe `Biblioteca` com uma lista de `Livro`. Implemente métodos: `adicionarLivro`, `buscarPorAutor`, `listarTodosOrdenados` (por título). Crie testes unitários para cada método.

**Exercício 2 — Separação de responsabilidades**

Refatore o seguinte código para separar a lógica da UI:

```java
// Refatore isto:
public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    ArrayList<Integer> nums = new ArrayList<>();
    while (true) {
        String l = sc.nextLine();
        if (l.equals("fim")) break;
        nums.add(Integer.valueOf(l));
    }
    int soma = 0;
    for (int n : nums) soma += n;
    System.out.println("Soma: " + soma);
    System.out.println("Média: " + (double) soma / nums.size());
}
```

Crie classes `ColetorDeNumeros` (lógica) e `InterfaceColetorDeNumeros` (UI).

**Exercício 3 — TDD: Conta bancária**

Usando TDD, implemente a classe `ContaBancaria` com os métodos `depositar`, `sacar` e `getSaldo`. Escreva os testes primeiro:
- `depositar` aumenta o saldo
- `sacar` com saldo suficiente diminui o saldo e retorna `true`
- `sacar` sem saldo suficiente não altera o saldo e retorna `false`
- Saldo nunca fica negativo

**Exercício 4 — Programa complexo: inventário**

Crie um sistema de inventário simples com:
- Classe `Item` (nome, quantidade, preço unitário)
- Classe `Inventario` (lista de itens, adicionar, remover, buscar, valor total)
- Classe `InterfaceInventario` (menu com opções: adicionar, remover, buscar, listar, total)
- Testes para a classe `Inventario`
