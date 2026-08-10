# Resoluções — Lição 4

Resolução dos 4 exercícios propostos em `licao04.md`. Em cada exercício, os arquivos `.java` estão organizados como estariam em um projeto real (uma classe por arquivo), mas aqui ficam todos juntos para facilitar a leitura.

---

## Exercício 1 — Retângulo

```java
public class Retangulo {
    private double largura;
    private double altura;

    public Retangulo(double largura, double altura) {
        this.largura = largura;
        this.altura = altura;
    }

    public double getArea() {
        return largura * altura;
    }

    public double getPerimetro() {
        return 2 * (largura + altura);
    }

    public boolean ehQuadrado() {
        return largura == altura;
    }

    @Override
    public String toString() {
        return "Retangulo{largura=" + largura + ", altura=" + altura +
               ", area=" + getArea() + ", perimetro=" + getPerimetro() + "}";
    }

    public static void main(String[] args) {
        Retangulo r1 = new Retangulo(4, 4);
        Retangulo r2 = new Retangulo(3, 5);

        System.out.println(r1);
        System.out.println("É quadrado? " + r1.ehQuadrado());

        System.out.println(r2);
        System.out.println("É quadrado? " + r2.ehQuadrado());
    }
}
```

---

## Exercício 2 — Aluno e notas

```java
import java.util.ArrayList;

public class Aluno {
    private String nome;
    private ArrayList<Integer> notas;

    public Aluno(String nome) {
        this.nome = nome;
        this.notas = new ArrayList<>();
    }

    public void adicionarNota(int nota) {
        if (nota < 0 || nota > 10) {
            throw new IllegalArgumentException("A nota deve estar entre 0 e 10.");
        }
        notas.add(nota);
    }

    public double calcularMedia() {
        if (notas.isEmpty()) {
            return 0;
        }
        int soma = 0;
        for (int nota : notas) {
            soma += nota;
        }
        return (double) soma / notas.size();
    }

    public boolean aprovado() {
        return calcularMedia() >= 6;
    }

    @Override
    public String toString() {
        String situacao = aprovado() ? "Aprovado" : "Reprovado";
        return nome + " - média: " + calcularMedia() + " - " + situacao;
    }

    public static void main(String[] args) {
        Aluno aluno = new Aluno("Pedro");
        aluno.adicionarNota(8);
        aluno.adicionarNota(7);
        aluno.adicionarNota(5);

        System.out.println(aluno);
    }
}
```

---

## Exercício 3 — Estacionamento

```java
public class Veiculo {
    private String placa;
    private String marca;

    public Veiculo(String placa, String marca) {
        this.placa = placa;
        this.marca = marca;
    }

    public String getPlaca() {
        return placa;
    }

    public String getMarca() {
        return marca;
    }

    @Override
    public String toString() {
        return marca + " - " + placa;
    }
}
```

```java
import java.util.ArrayList;

public class Estacionamento {
    private int capacidade;
    private ArrayList<Veiculo> veiculos;

    public Estacionamento(int capacidade) {
        this.capacidade = capacidade;
        this.veiculos = new ArrayList<>();
    }

    public boolean entrar(Veiculo v) {
        if (veiculos.size() >= capacidade) {
            System.out.println("Estacionamento lotado. Não é possível entrar.");
            return false;
        }
        veiculos.add(v);
        return true;
    }

    public boolean sair(String placa) {
        for (Veiculo v : veiculos) {
            if (v.getPlaca().equals(placa)) {
                veiculos.remove(v);
                return true;
            }
        }
        return false;
    }

    public void listarVeiculos() {
        System.out.println("Veículos no estacionamento (" + veiculos.size() + "/" + capacidade + "):");
        for (Veiculo v : veiculos) {
            System.out.println("- " + v);
        }
    }

    public static void main(String[] args) {
        Estacionamento estacionamento = new Estacionamento(2);

        estacionamento.entrar(new Veiculo("ABC1234", "Fiat"));
        estacionamento.entrar(new Veiculo("XYZ9876", "Toyota"));
        estacionamento.entrar(new Veiculo("QWE4567", "Ford")); // lotado

        estacionamento.listarVeiculos();

        estacionamento.sair("ABC1234");
        estacionamento.listarVeiculos();
    }
}
```

---

## Exercício 4 — Leitura de arquivo

Primeiro, criamos o arquivo `produtos.txt` com linhas no formato `nome,preco,estoque`:

```
Caneta,2.50,100
Caderno,15.90,40
Mochila,89.90,12
Estojo,22.30,25
```

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

    public double getValorTotal() {
        return preco * estoque;
    }

    @Override
    public String toString() {
        return nome + " - R$" + preco + " - estoque: " + estoque;
    }
}
```

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.util.ArrayList;

public class LeitorDeProdutos {
    public static void main(String[] args) {
        ArrayList<Produto> produtos = new ArrayList<>();

        try (BufferedReader leitor = new BufferedReader(new FileReader("produtos.txt"))) {
            String linha;
            while ((linha = leitor.readLine()) != null) {
                String[] partes = linha.split(",");

                String nome = partes[0];
                double preco = Double.valueOf(partes[1]);
                int estoque = Integer.valueOf(partes[2]);

                produtos.add(new Produto(nome, preco, estoque));
            }
        } catch (IOException e) {
            System.out.println("Erro ao ler o arquivo: " + e.getMessage());
            return;
        }

        double valorTotal = 0;

        System.out.println("Produtos cadastrados:");
        for (Produto produto : produtos) {
            System.out.println(produto);
            valorTotal += produto.getValorTotal();
        }

        System.out.println("\nValor total em estoque: R$" + valorTotal);
    }
}
```

O uso do `try-with-resources` garante que o arquivo seja fechado automaticamente mesmo se ocorrer um erro durante a leitura.
