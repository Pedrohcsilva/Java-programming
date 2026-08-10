# Resoluções — Lição 6

Resolução dos 4 exercícios propostos em `licao06.md`. Os testes usam JUnit 4, do mesmo jeito que foi apresentado na lição (`org.junit.Test` e `org.junit.Assert`).

---

## Exercício 1 — Biblioteca

```java
public class Livro {
    private String titulo;
    private String autor;

    public Livro(String titulo, String autor) {
        this.titulo = titulo;
        this.autor = autor;
    }

    public String getTitulo() {
        return titulo;
    }

    public String getAutor() {
        return autor;
    }

    @Override
    public String toString() {
        return titulo + " - " + autor;
    }
}
```

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;

public class Biblioteca {
    private ArrayList<Livro> livros;

    public Biblioteca() {
        this.livros = new ArrayList<>();
    }

    public void adicionarLivro(Livro livro) {
        livros.add(livro);
    }

    public ArrayList<Livro> buscarPorAutor(String autor) {
        ArrayList<Livro> encontrados = new ArrayList<>();
        for (Livro livro : livros) {
            if (livro.getAutor().equals(autor)) {
                encontrados.add(livro);
            }
        }
        return encontrados;
    }

    public ArrayList<Livro> listarTodosOrdenados() {
        ArrayList<Livro> ordenados = new ArrayList<>(livros);
        Collections.sort(ordenados, Comparator.comparing(Livro::getTitulo));
        return ordenados;
    }
}
```

```java
import org.junit.Before;
import org.junit.Test;
import static org.junit.Assert.*;

public class BibliotecaTest {

    private Biblioteca biblioteca;

    @Before
    public void setUp() {
        biblioteca = new Biblioteca();
        biblioteca.adicionarLivro(new Livro("Torto Arado", "Itamar Vieira Junior"));
        biblioteca.adicionarLivro(new Livro("Dom Casmurro", "Machado de Assis"));
        biblioteca.adicionarLivro(new Livro("Quincas Borba", "Machado de Assis"));
    }

    @Test
    public void adicionarLivroAumentaTamanho() {
        Biblioteca vazia = new Biblioteca();
        assertEquals(0, vazia.listarTodosOrdenados().size());

        vazia.adicionarLivro(new Livro("1984", "George Orwell"));
        assertEquals(1, vazia.listarTodosOrdenados().size());
    }

    @Test
    public void buscarPorAutorRetornaTodosOsLivrosDoAutor() {
        assertEquals(2, biblioteca.buscarPorAutor("Machado de Assis").size());
        assertEquals(1, biblioteca.buscarPorAutor("Itamar Vieira Junior").size());
        assertEquals(0, biblioteca.buscarPorAutor("Autor Inexistente").size());
    }

    @Test
    public void listarTodosOrdenadosRetornaEmOrdemAlfabetica() {
        var ordenados = biblioteca.listarTodosOrdenados();

        assertEquals("Dom Casmurro", ordenados.get(0).getTitulo());
        assertEquals("Quincas Borba", ordenados.get(1).getTitulo());
        assertEquals("Torto Arado", ordenados.get(2).getTitulo());
    }
}
```

---

## Exercício 2 — Separação de responsabilidades

```java
import java.util.ArrayList;

public class ColetorDeNumeros {
    private ArrayList<Integer> numeros;

    public ColetorDeNumeros() {
        this.numeros = new ArrayList<>();
    }

    public void adicionar(int numero) {
        numeros.add(numero);
    }

    public int soma() {
        int soma = 0;
        for (int n : numeros) {
            soma += n;
        }
        return soma;
    }

    public double media() {
        if (numeros.isEmpty()) {
            return 0;
        }
        return (double) soma() / numeros.size();
    }
}
```

```java
import java.util.Scanner;

public class InterfaceColetorDeNumeros {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        ColetorDeNumeros coletor = new ColetorDeNumeros();

        while (true) {
            String linha = scanner.nextLine();
            if (linha.equals("fim")) {
                break;
            }
            coletor.adicionar(Integer.valueOf(linha));
        }

        System.out.println("Soma: " + coletor.soma());
        System.out.println("Média: " + coletor.media());
    }
}
```

Com essa separação, `ColetorDeNumeros` pode ser testada com JUnit sem precisar simular entrada de usuário — toda a lógica de soma e média fica isolada da leitura via `Scanner`.

---

## Exercício 3 — TDD: Conta bancária

Seguindo TDD, os testes vêm primeiro:

```java
import org.junit.Before;
import org.junit.Test;
import static org.junit.Assert.*;

public class ContaBancariaTest {

    private ContaBancaria conta;

    @Before
    public void setUp() {
        conta = new ContaBancaria();
    }

    @Test
    public void depositarAumentaSaldo() {
        conta.depositar(100);
        assertEquals(100, conta.getSaldo(), 0.001);
    }

    @Test
    public void sacarComSaldoSuficienteDiminuiSaldoERetornaTrue() {
        conta.depositar(100);

        boolean resultado = conta.sacar(40);

        assertTrue(resultado);
        assertEquals(60, conta.getSaldo(), 0.001);
    }

    @Test
    public void sacarSemSaldoSuficienteNaoAlteraSaldoERetornaFalse() {
        conta.depositar(50);

        boolean resultado = conta.sacar(100);

        assertFalse(resultado);
        assertEquals(50, conta.getSaldo(), 0.001);
    }

    @Test
    public void saldoNuncaFicaNegativo() {
        conta.sacar(500); // conta começa com saldo 0
        assertTrue(conta.getSaldo() >= 0);
    }
}
```

E só então a implementação, feita para satisfazer os testes acima:

```java
public class ContaBancaria {
    private double saldo;

    public ContaBancaria() {
        this.saldo = 0;
    }

    public void depositar(double valor) {
        saldo += valor;
    }

    public boolean sacar(double valor) {
        if (valor > saldo) {
            return false;
        }
        saldo -= valor;
        return true;
    }

    public double getSaldo() {
        return saldo;
    }
}
```

---

## Exercício 4 — Programa complexo: inventário

```java
public class Item {
    private String nome;
    private int quantidade;
    private double precoUnitario;

    public Item(String nome, int quantidade, double precoUnitario) {
        this.nome = nome;
        this.quantidade = quantidade;
        this.precoUnitario = precoUnitario;
    }

    public String getNome() {
        return nome;
    }

    public int getQuantidade() {
        return quantidade;
    }

    public void setQuantidade(int quantidade) {
        this.quantidade = quantidade;
    }

    public double getValorTotal() {
        return quantidade * precoUnitario;
    }

    @Override
    public String toString() {
        return nome + " (qtd: " + quantidade + ", unit: R$" + precoUnitario + ")";
    }
}
```

```java
import java.util.ArrayList;

public class Inventario {
    private ArrayList<Item> itens;

    public Inventario() {
        this.itens = new ArrayList<>();
    }

    public void adicionar(Item item) {
        itens.add(item);
    }

    public boolean remover(String nome) {
        Item item = buscar(nome);
        if (item == null) {
            return false;
        }
        itens.remove(item);
        return true;
    }

    public Item buscar(String nome) {
        for (Item item : itens) {
            if (item.getNome().equals(nome)) {
                return item;
            }
        }
        return null;
    }

    public double valorTotal() {
        double total = 0;
        for (Item item : itens) {
            total += item.getValorTotal();
        }
        return total;
    }

    public ArrayList<Item> listar() {
        return itens;
    }
}
```

```java
import org.junit.Before;
import org.junit.Test;
import static org.junit.Assert.*;

public class InventarioTest {

    private Inventario inventario;

    @Before
    public void setUp() {
        inventario = new Inventario();
        inventario.adicionar(new Item("Parafuso", 100, 0.10));
        inventario.adicionar(new Item("Martelo", 5, 25.00));
    }

    @Test
    public void buscarEncontraItemExistente() {
        assertNotNull(inventario.buscar("Martelo"));
        assertNull(inventario.buscar("Chave de fenda"));
    }

    @Test
    public void removerRetornaTrueQuandoItemExiste() {
        assertTrue(inventario.remover("Parafuso"));
        assertNull(inventario.buscar("Parafuso"));
    }

    @Test
    public void removerRetornaFalseQuandoItemNaoExiste() {
        assertFalse(inventario.remover("Item Fantasma"));
    }

    @Test
    public void valorTotalSomaCorretamenteTodosOsItens() {
        // 100 * 0.10 + 5 * 25.00 = 10 + 125 = 135
        assertEquals(135.0, inventario.valorTotal(), 0.001);
    }
}
```

```java
import java.util.Scanner;

public class InterfaceInventario {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Inventario inventario = new Inventario();

        while (true) {
            System.out.println("\n1-Adicionar 2-Remover 3-Buscar 4-Listar 5-Total 6-Sair");
            String opcao = scanner.nextLine();

            switch (opcao) {
                case "1":
                    System.out.print("Nome: ");
                    String nome = scanner.nextLine();
                    System.out.print("Quantidade: ");
                    int quantidade = Integer.valueOf(scanner.nextLine());
                    System.out.print("Preço unitário: ");
                    double preco = Double.valueOf(scanner.nextLine());
                    inventario.adicionar(new Item(nome, quantidade, preco));
                    break;
                case "2":
                    System.out.print("Nome do item a remover: ");
                    boolean removido = inventario.remover(scanner.nextLine());
                    System.out.println(removido ? "Removido." : "Item não encontrado.");
                    break;
                case "3":
                    System.out.print("Nome do item a buscar: ");
                    Item encontrado = inventario.buscar(scanner.nextLine());
                    System.out.println(encontrado != null ? encontrado : "Item não encontrado.");
                    break;
                case "4":
                    for (Item item : inventario.listar()) {
                        System.out.println(item);
                    }
                    break;
                case "5":
                    System.out.println("Valor total: R$" + inventario.valorTotal());
                    break;
                case "6":
                    return;
                default:
                    System.out.println("Opção inválida.");
            }
        }
    }
}
```
