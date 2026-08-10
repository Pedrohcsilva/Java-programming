# Resoluções — Lição 2

Resolução dos 6 exercícios propostos em `licao02.md`.

---

## Exercício 1 — Tabuada

```java
import java.util.Scanner;

public class Tabuada {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Digite um número: ");
        int n = Integer.valueOf(scanner.nextLine());

        for (int i = 1; i <= 10; i++) {
            System.out.println(n + " x " + i + " = " + (n * i));
        }
    }
}
```

---

## Exercício 2 — Fatorial

```java
public class Fatorial {

    public static int fatorial(int n) {
        int resultado = 1;
        for (int i = 2; i <= n; i++) {
            resultado *= i;
        }
        return resultado;
    }

    public static void main(String[] args) {
        System.out.println("fatorial(0) = " + fatorial(0));
        System.out.println("fatorial(1) = " + fatorial(1));
        System.out.println("fatorial(5) = " + fatorial(5));
        System.out.println("fatorial(10) = " + fatorial(10));
    }
}
```

O laço começa em `i = 2` porque multiplicar por 1 não altera o resultado. Para `n = 0` e `n = 1`, o laço nunca executa e a função retorna `1`, que é o valor correto do fatorial nesses dois casos.

---

## Exercício 3 — Contagem de pares e ímpares

```java
import java.util.Scanner;

public class ContagemParesImpares {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int pares = 0;
        int impares = 0;

        while (true) {
            System.out.print("Digite um número (0 para parar): ");
            int numero = Integer.valueOf(scanner.nextLine());

            if (numero == 0) {
                break;
            }

            if (numero % 2 == 0) {
                pares++;
            } else {
                impares++;
            }
        }

        System.out.println("Pares: " + pares);
        System.out.println("Ímpares: " + impares);
    }
}
```

---

## Exercício 4 — Potência

```java
public class Potencia {

    public static int potencia(int base, int expoente) {
        int resultado = 1;
        for (int i = 0; i < expoente; i++) {
            resultado *= base;
        }
        return resultado;
    }

    public static void main(String[] args) {
        System.out.println(potencia(2, 10));
    }
}
```

---

## Exercício 5 — Validação de entrada

```java
import java.util.Scanner;

public class ValidacaoEntrada {

    public static int lerInteiroPositivo(Scanner scanner) {
        while (true) {
            System.out.print("Digite um número positivo: ");
            int numero = Integer.valueOf(scanner.nextLine());

            if (numero > 0) {
                return numero;
            }

            System.out.println("Erro: o número precisa ser positivo. Tente novamente.");
        }
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        int a = lerInteiroPositivo(scanner);
        int b = lerInteiroPositivo(scanner);

        System.out.println("Produto: " + (a * b));
    }
}
```

---

## Exercício 6 — Estrelas

```java
public class Estrelas {

    public static void imprimirEstrelas(int n) {
        String linha = "";
        for (int i = 0; i < n; i++) {
            linha += "*";
        }
        System.out.println(linha);
    }

    public static void main(String[] args) {
        for (int i = 1; i <= 5; i++) {
            imprimirEstrelas(i);
        }
    }
}
```

**Saída:**
```
*
**
***
****
*****
```
