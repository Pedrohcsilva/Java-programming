# Resoluções — Lição 3

Resolução dos 5 exercícios propostos em `licao03.md`.

---

## Exercício 1 — Lista de compras

```java
import java.util.ArrayList;
import java.util.Scanner;

public class ListaDeCompras {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        ArrayList<String> itens = new ArrayList<>();

        while (true) {
            System.out.print("Item (ou 'sair'): ");
            String item = scanner.nextLine();

            if (item.equals("sair")) {
                break;
            }

            itens.add(item);
        }

        System.out.println("\nLista de compras:");
        for (int i = 0; i < itens.size(); i++) {
            System.out.println((i + 1) + ". " + itens.get(i));
        }

        System.out.println("Total de itens: " + itens.size());
    }
}
```

---

## Exercício 2 — Mínimo e máximo

```java
import java.util.ArrayList;
import java.util.Scanner;

public class MinimoMaximo {

    public static int minimo(ArrayList<Integer> lista) {
        int menor = lista.get(0);
        for (int numero : lista) {
            if (numero < menor) {
                menor = numero;
            }
        }
        return menor;
    }

    public static int maximo(ArrayList<Integer> lista) {
        int maior = lista.get(0);
        for (int numero : lista) {
            if (numero > maior) {
                maior = numero;
            }
        }
        return maior;
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        ArrayList<Integer> numeros = new ArrayList<>();

        for (int i = 0; i < 6; i++) {
            System.out.print("Número " + (i + 1) + ": ");
            numeros.add(Integer.valueOf(scanner.nextLine()));
        }

        System.out.println("Menor: " + minimo(numeros));
        System.out.println("Maior: " + maximo(numeros));
    }
}
```

---

## Exercício 3 — Palavras únicas

```java
import java.util.ArrayList;
import java.util.Scanner;

public class PalavrasUnicas {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        ArrayList<String> palavras = new ArrayList<>();

        while (true) {
            System.out.print("Palavra (ou 'fim'): ");
            String palavra = scanner.nextLine();

            if (palavra.equals("fim")) {
                break;
            }

            if (!palavras.contains(palavra)) {
                palavras.add(palavra);
            }
        }

        System.out.println("\nPalavras únicas:");
        for (String palavra : palavras) {
            System.out.println(palavra);
        }
    }
}
```

`ArrayList.contains` já usa `equals` internamente, então basta checar `!palavras.contains(palavra)` antes de adicionar.

---

## Exercício 4 — Invertendo um array

```java
public class InverterArray {

    public static int[] inverter(int[] arr) {
        int[] invertido = new int[arr.length];

        for (int i = 0; i < arr.length; i++) {
            invertido[i] = arr[arr.length - 1 - i];
        }

        return invertido;
    }

    public static void main(String[] args) {
        int[] original = {1, 2, 3, 4, 5};
        int[] invertido = inverter(original);

        System.out.print("Original: ");
        for (int n : original) System.out.print(n + " ");

        System.out.print("\nInvertido: ");
        for (int n : invertido) System.out.print(n + " ");
        System.out.println();
    }
}
```

Como o método cria um array novo (`invertido`) e apenas lê o `arr` original, o array passado como parâmetro não é modificado.

---

## Exercício 5 — Análise de frase

```java
import java.util.Scanner;

public class AnaliseFrase {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Digite uma frase: ");
        String frase = scanner.nextLine();

        String[] palavras = frase.split(" ");

        String maisLonga = palavras[0];
        String maisCurta = palavras[0];

        for (String palavra : palavras) {
            if (palavra.length() > maisLonga.length()) {
                maisLonga = palavra;
            }
            if (palavra.length() < maisCurta.length()) {
                maisCurta = palavra;
            }
        }

        System.out.println("Número de palavras: " + palavras.length);
        System.out.println("Palavra mais longa: " + maisLonga);
        System.out.println("Palavra mais curta: " + maisCurta);
        System.out.println("Em maiúsculo: " + frase.toUpperCase());
    }
}
```
