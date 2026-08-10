# Resoluções — Lição 7

Resolução dos 3 exercícios finais propostos em `licao07.md`. Como o enunciado não fixa uma estrutura de classes, as escolhas de design abaixo são uma entre várias soluções possíveis.

---

## Exercício 1 — Agenda de contatos

```java
import java.util.Objects;

public class Contato {
    private String nome;
    private String telefone;
    private String email;

    public Contato(String nome, String telefone, String email) {
        this.nome = nome;
        this.telefone = telefone;
        this.email = email;
    }

    public String getNome() {
        return nome;
    }

    public String getTelefone() {
        return telefone;
    }

    public String getEmail() {
        return email;
    }

    @Override
    public String toString() {
        return nome + " | " + telefone + " | " + email;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (!(obj instanceof Contato)) return false;
        return nome.equals(((Contato) obj).nome);
    }

    @Override
    public int hashCode() {
        return Objects.hash(nome);
    }
}
```

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;

public class Agenda {
    private ArrayList<Contato> contatos;

    public Agenda() {
        this.contatos = new ArrayList<>();
    }

    public void adicionarContato(Contato c) {
        contatos.add(c);
    }

    public ArrayList<Contato> buscarPorNome(String nome) {
        ArrayList<Contato> encontrados = new ArrayList<>();
        String busca = nome.toLowerCase();

        for (Contato c : contatos) {
            if (c.getNome().toLowerCase().contains(busca)) {
                encontrados.add(c);
            }
        }
        return encontrados;
    }

    public boolean removerContato(String nome) {
        for (Contato c : contatos) {
            if (c.getNome().equals(nome)) {
                contatos.remove(c);
                return true;
            }
        }
        return false;
    }

    public ArrayList<Contato> listarTodos() {
        ArrayList<Contato> ordenados = new ArrayList<>(contatos);
        Collections.sort(ordenados, Comparator.comparing(Contato::getNome));
        return ordenados;
    }
}
```

```java
import java.util.Scanner;

public class InterfaceAgenda {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Agenda agenda = new Agenda();

        while (true) {
            System.out.println("\n1-Adicionar 2-Buscar 3-Remover 4-Listar 5-Sair");
            String opcao = scanner.nextLine();

            switch (opcao) {
                case "1":
                    System.out.print("Nome: ");
                    String nome = scanner.nextLine();
                    System.out.print("Telefone: ");
                    String telefone = scanner.nextLine();
                    System.out.print("Email: ");
                    String email = scanner.nextLine();
                    agenda.adicionarContato(new Contato(nome, telefone, email));
                    break;
                case "2":
                    System.out.print("Buscar por nome: ");
                    for (Contato c : agenda.buscarPorNome(scanner.nextLine())) {
                        System.out.println(c);
                    }
                    break;
                case "3":
                    System.out.print("Nome do contato a remover: ");
                    boolean removido = agenda.removerContato(scanner.nextLine());
                    System.out.println(removido ? "Removido." : "Contato não encontrado.");
                    break;
                case "4":
                    for (Contato c : agenda.listarTodos()) {
                        System.out.println(c);
                    }
                    break;
                case "5":
                    return;
                default:
                    System.out.println("Opção inválida.");
            }
        }
    }
}
```

---

## Exercício 2 — Registro de notas

```java
import java.util.ArrayList;
import java.util.Collections;

public class Aluno {
    private String nome;
    private String matricula;
    private ArrayList<Integer> notas;

    public Aluno(String nome, String matricula) {
        this.nome = nome;
        this.matricula = matricula;
        this.notas = new ArrayList<>();
    }

    public String getNome() {
        return nome;
    }

    public String getMatricula() {
        return matricula;
    }

    public void adicionarNota(int nota) {
        if (nota < 0 || nota > 10) {
            throw new IllegalArgumentException("A nota deve estar entre 0 e 10.");
        }
        notas.add(nota);
    }

    public double calcularMedia() {
        if (notas.isEmpty()) return 0;
        int soma = 0;
        for (int n : notas) soma += n;
        return (double) soma / notas.size();
    }

    public int melhorNota() {
        return Collections.max(notas);
    }

    public int piorNota() {
        return Collections.min(notas);
    }

    public boolean aprovado(double notaMinima) {
        return calcularMedia() >= notaMinima;
    }

    @Override
    public String toString() {
        return nome + " (" + matricula + ") - média: " + calcularMedia();
    }
}
```

```java
import java.util.ArrayList;

public class Turma {
    private ArrayList<Aluno> alunos;

    public Turma() {
        this.alunos = new ArrayList<>();
    }

    public void adicionarAluno(Aluno aluno) {
        alunos.add(aluno);
    }

    public Aluno buscarPorMatricula(String matricula) {
        for (Aluno a : alunos) {
            if (a.getMatricula().equals(matricula)) {
                return a;
            }
        }
        return null;
    }

    public double mediaGeral() {
        if (alunos.isEmpty()) return 0;
        double soma = 0;
        for (Aluno a : alunos) {
            soma += a.calcularMedia();
        }
        return soma / alunos.size();
    }

    public Aluno melhorAluno() {
        Aluno melhor = alunos.get(0);
        for (Aluno a : alunos) {
            if (a.calcularMedia() > melhor.calcularMedia()) {
                melhor = a;
            }
        }
        return melhor;
    }

    public ArrayList<Aluno> listarAprovados(double notaMinima) {
        ArrayList<Aluno> aprovados = new ArrayList<>();
        for (Aluno a : alunos) {
            if (a.aprovado(notaMinima)) {
                aprovados.add(a);
            }
        }
        return aprovados;
    }

    public static void main(String[] args) {
        Turma turma = new Turma();

        String[] nomes = {"Ana", "Bruno", "Carla", "Diego", "Elisa"};
        int[][] notasPorAluno = {
            {8, 7, 9},
            {5, 4, 6},
            {10, 9, 10},
            {3, 5, 4},
            {7, 8, 6}
        };

        for (int i = 0; i < nomes.length; i++) {
            Aluno aluno = new Aluno(nomes[i], "M0" + (i + 1));
            for (int nota : notasPorAluno[i]) {
                aluno.adicionarNota(nota);
            }
            turma.adicionarAluno(aluno);
        }

        System.out.println("Média geral da turma: " + turma.mediaGeral());
        System.out.println("Melhor aluno: " + turma.melhorAluno());

        System.out.println("Aprovados (média >= 6):");
        for (Aluno aprovado : turma.listarAprovados(6)) {
            System.out.println(aprovado);
        }
    }
}
```

---

## Exercício 3 — Loja simples

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

    public String getNome() {
        return nome;
    }

    public double getPreco() {
        return preco;
    }

    public int getEstoque() {
        return estoque;
    }

    public boolean vender(int qtd) {
        if (qtd > estoque) {
            return false;
        }
        estoque -= qtd;
        return true;
    }

    public void repor(int qtd) {
        estoque += qtd;
    }

    @Override
    public String toString() {
        return nome + " - R$" + preco + " (estoque: " + estoque + ")";
    }
}
```

```java
import java.util.HashMap;
import java.util.Map;

public class Carrinho {
    private HashMap<Produto, Integer> itens;

    public Carrinho() {
        this.itens = new HashMap<>();
    }

    public void adicionar(Produto produto, int qtd) {
        itens.put(produto, itens.getOrDefault(produto, 0) + qtd);
    }

    public void remover(Produto produto) {
        itens.remove(produto);
    }

    public double calcularTotal() {
        double total = 0;
        for (Map.Entry<Produto, Integer> entrada : itens.entrySet()) {
            total += entrada.getKey().getPreco() * entrada.getValue();
        }
        return total;
    }

    public HashMap<Produto, Integer> getItens() {
        return itens;
    }
}
```

```java
import java.util.ArrayList;
import java.util.Map;

public class Loja {
    private ArrayList<Produto> produtos;

    public Loja() {
        this.produtos = new ArrayList<>();
    }

    public void adicionarProduto(Produto produto) {
        produtos.add(produto);
    }

    public Produto buscarProduto(String nome) {
        for (Produto p : produtos) {
            if (p.getNome().equals(nome)) {
                return p;
            }
        }
        return null;
    }

    public boolean finalizarVenda(Carrinho carrinho) {
        // primeiro verifica se há estoque suficiente para tudo
        for (Map.Entry<Produto, Integer> entrada : carrinho.getItens().entrySet()) {
            if (entrada.getValue() > entrada.getKey().getEstoque()) {
                System.out.println("Estoque insuficiente para: " + entrada.getKey().getNome());
                return false;
            }
        }

        // só então efetiva a baixa no estoque de cada produto
        for (Map.Entry<Produto, Integer> entrada : carrinho.getItens().entrySet()) {
            entrada.getKey().vender(entrada.getValue());
        }

        System.out.println("Venda finalizada. Total: R$" + carrinho.calcularTotal());
        return true;
    }

    public static void main(String[] args) {
        Loja loja = new Loja();

        Produto caneta = new Produto("Caneta", 2.50, 100);
        Produto caderno = new Produto("Caderno", 15.90, 40);
        Produto mochila = new Produto("Mochila", 89.90, 12);

        loja.adicionarProduto(caneta);
        loja.adicionarProduto(caderno);
        loja.adicionarProduto(mochila);

        Carrinho carrinho = new Carrinho();
        carrinho.adicionar(loja.buscarProduto("Caneta"), 3);
        carrinho.adicionar(loja.buscarProduto("Caderno"), 2);
        carrinho.adicionar(loja.buscarProduto("Mochila"), 1);

        loja.finalizarVenda(carrinho);

        System.out.println("Estoque de caneta após a venda: " + caneta.getEstoque());
    }
}
```

**Observação:** `Produto` é usado como chave de um `HashMap` sem sobrescrever `equals`/`hashCode`, então a comparação usada pelo `Carrinho` é por identidade de objeto (o mesmo objeto `Produto`, não produtos "iguais" por nome). Isso é aceitável aqui porque a `Loja` sempre devolve a mesma instância via `buscarProduto`.
