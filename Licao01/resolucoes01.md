# Resoluções — Lição 1: Primeiros Passos

Resolução dos 5 exercícios propostos em `licao01.md`.

---

## Exercício 1 — Olá, usuário!

```java
import java.util.Scanner;

public class OlaUsuario {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Qual é o seu nome? ");
        String nome = scanner.nextLine();

        System.out.print("Qual é a sua idade? ");
        int idade = Integer.valueOf(scanner.nextLine());

        System.out.println("Olá, " + nome + "! Você tem " + idade + " anos.");
    }
}
```

**Exemplo de execução:**
```
Qual é o seu nome? Pedro
Qual é a sua idade? 21
Olá, Pedro! Você tem 21 anos.
```

---

## Exercício 2 — Calculadora simples

```java
import java.util.Scanner;

public class CalculadoraSimples {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Primeiro número: ");
        int a = Integer.valueOf(scanner.nextLine());

        System.out.print("Segundo número: ");
        int b = Integer.valueOf(scanner.nextLine());

        double quociente = (double) a / b;

        System.out.println("Soma: " + (a + b));
        System.out.println("Diferença: " + (a - b));
        System.out.println("Produto: " + (a * b));
        System.out.println("Quociente: " + quociente);
    }
}
```

O quociente usa `(double) a / b` para não perder a parte decimal do resultado.

---

## Exercício 3 — Par ou ímpar

```java
import java.util.Scanner;

public class ParOuImpar {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Digite um número inteiro: ");
        int numero = Integer.valueOf(scanner.nextLine());

        if (numero % 2 == 0) {
            System.out.println(numero + " é par.");
        } else {
            System.out.println(numero + " é ímpar.");
        }
    }
}
```

---

## Exercício 4 — Classificação de temperatura

```java
import java.util.Scanner;

public class ClassificacaoTemperatura {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Digite a temperatura em °C: ");
        double temperatura = Double.valueOf(scanner.nextLine());

        if (temperatura < 15) {
            System.out.println("Frio");
        } else if (temperatura <= 25) {
            System.out.println("Agradável");
        } else {
            System.out.println("Quente");
        }
    }
}
```

---

## Exercício 5 — IMC

```java
import java.util.Scanner;

public class CalculadoraIMC {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Peso (kg): ");
        double peso = Double.valueOf(scanner.nextLine());

        System.out.print("Altura (m): ");
        double altura = Double.valueOf(scanner.nextLine());

        double imc = peso / (altura * altura);

        System.out.println("Seu IMC é: " + imc);

        if (imc < 18.5) {
            System.out.println("Classificação: Abaixo do peso");
        } else if (imc < 25) {
            System.out.println("Classificação: Peso normal");
        } else if (imc < 30) {
            System.out.println("Classificação: Sobrepeso");
        } else {
            System.out.println("Classificação: Obesidade");
        }
    }
}
```

**Observação:** os limites usam `<` (estrito) em vez de `<=` para os intervalos, seguindo exatamente as faixas descritas no enunciado (ex.: 24.9 já cai em "abaixo de 25").
