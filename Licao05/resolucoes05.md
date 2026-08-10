# Resoluções — Lição 5

Resolução dos 4 exercícios propostos em `licao05.md`.

---

## Exercício 1 — Sobrecarga de construtores

```java
import java.util.Objects;

public class Livro {
    private String titulo;
    private String autor;
    private int anoPublicacao;

    public Livro(String titulo, String autor, int anoPublicacao) {
        this.titulo = titulo;
        this.autor = autor;
        this.anoPublicacao = anoPublicacao;
    }

    public Livro(String titulo, String autor) {
        this(titulo, autor, 0);
    }

    public Livro(String titulo) {
        this(titulo, "Desconhecido", 0);
    }

    @Override
    public String toString() {
        return titulo + " - " + autor + " (" + anoPublicacao + ")";
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (!(obj instanceof Livro)) return false;

        Livro outro = (Livro) obj;
        return titulo.equals(outro.titulo) && autor.equals(outro.autor);
    }

    @Override
    public int hashCode() {
        return Objects.hash(titulo, autor);
    }

    public static void main(String[] args) {
        Livro l1 = new Livro("Dom Casmurro", "Machado de Assis", 1899);
        Livro l2 = new Livro("O Cortiço", "Aluísio Azevedo");
        Livro l3 = new Livro("Sem título definido");
        Livro l4 = new Livro("Dom Casmurro", "Machado de Assis", 2020);

        System.out.println(l1);
        System.out.println(l2);
        System.out.println(l3);

        System.out.println("l1 equals l4? " + l1.equals(l4)); // true, mesmo com ano diferente
    }
}
```

O uso de `this(...)` no início dos construtores encurtados evita repetir a lógica de atribuição — cada construtor "cai" no construtor mais completo passando os valores padrão que faltam.

---

## Exercício 2 — Referências vs. primitivas

```java
public class ReferenciasVsPrimitivas {

    static class Contador {
        int valor;

        Contador(int valor) {
            this.valor = valor;
        }
    }

    public static void main(String[] args) {
        // Copiando um tipo primitivo (int)
        int a = 10;
        int b = a; // b recebe uma CÓPIA do valor de a

        b = 99;

        System.out.println("--- Primitivos ---");
        System.out.println("a = " + a); // continua 10
        System.out.println("b = " + b); // 99

        // Copiando uma referência a um objeto
        Contador c1 = new Contador(10);
        Contador c2 = c1; // c2 aponta para o MESMO objeto que c1

        c2.valor = 99;

        System.out.println("--- Objetos ---");
        System.out.println("c1.valor = " + c1.valor); // também vira 99!
        System.out.println("c2.valor = " + c2.valor); // 99
    }
}
```

**Saída:**
```
--- Primitivos ---
a = 10
b = 99
--- Objetos ---
c1.valor = 99
c2.valor = 99
```

Isso mostra a diferença fundamental: `int` é copiado por valor, enquanto uma variável de objeto guarda uma referência (um "endereço"), então `c1` e `c2` acabam apontando para o mesmo espaço na memória.

---

## Exercício 3 — Ponto no plano

```java
public class Ponto {
    private double x;
    private double y;

    public Ponto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }

    public double distanciaAte(Ponto outro) {
        double dx = this.x - outro.x;
        double dy = this.y - outro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (!(obj instanceof Ponto)) return false;

        Ponto outro = (Ponto) obj;
        return Math.abs(this.x - outro.x) < 0.001 && Math.abs(this.y - outro.y) < 0.001;
    }

    @Override
    public String toString() {
        return "(" + x + ", " + y + ")";
    }
}
```

```java
public class Circulo {
    private Ponto centro;
    private double raio;

    public Circulo(Ponto centro, double raio) {
        this.centro = centro;
        this.raio = raio;
    }

    public boolean contemPonto(Ponto p) {
        return centro.distanciaAte(p) <= raio;
    }

    @Override
    public String toString() {
        return "Circulo{centro=" + centro + ", raio=" + raio + "}";
    }

    public static void main(String[] args) {
        Ponto centro = new Ponto(0, 0);
        Circulo circulo = new Circulo(centro, 5);

        Ponto dentro = new Ponto(3, 4); // distância = 5, está na borda
        Ponto fora = new Ponto(10, 10);

        System.out.println(circulo);
        System.out.println("(3,4) está dentro? " + circulo.contemPonto(dentro));
        System.out.println("(10,10) está dentro? " + circulo.contemPonto(fora));
    }
}
```

---

## Exercício 4 — Null safety

```java
public class Pessoa {
    private String nome;
    private int idade;

    public Pessoa(String nome, int idade) {
        this.nome = nome;
        this.idade = idade;
    }

    public String getNome() {
        return nome;
    }

    public int getIdade() {
        return idade;
    }
}
```

```java
public class NullSafety {

    public static void imprimirPessoa(Pessoa p) {
        if (p == null) {
            System.out.println("Pessoa não encontrada.");
            return;
        }
        System.out.println(p.getNome() + ", " + p.getIdade() + " anos");
    }

    public static void main(String[] args) {
        Pessoa pessoa = new Pessoa("Maria", 30);

        imprimirPessoa(pessoa);
        imprimirPessoa(null);
    }
}
```

**Saída:**
```
Maria, 30 anos
Pessoa não encontrada.
```
