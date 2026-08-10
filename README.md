# Java Programming I — Tradução e Resoluções (PT-BR)

Tradução para o português do curso **Java Programming I**, do [Java Programming MOOC](https://java-programming.mooc.fi/) da **Universidade de Helsinque**, acompanhada das resoluções comentadas dos exercícios propostos em cada lição.

> 🎓 Este é um **projeto de extensão** desenvolvido por alunos do curso de **Engenharia de Computação da Universidade Federal de Uberlândia (UFU)**.

## 📑 Sumário

- [Sobre](#-sobre)
- [Estrutura do repositório](#-estrutura-do-repositório)
- [Conteúdo das lições](#-conteúdo-das-lições)
- [Resoluções](#-resoluções)
- [Como usar](#-como-usar)
- [Como compilar os arquivos LaTeX](#️-como-compilar-os-arquivos-latex)
- [Fonte original e créditos](#-fonte-original-e-créditos)

## 📖 Sobre

Este repositório reúne o conteúdo teórico das 7 lições do curso, traduzido e adaptado para o português, junto com uma pasta dedicada às resoluções dos exercícios de cada lição. Cada material está disponível em três formatos: **Markdown**, **LaTeX** e **HTML**.

## 📂 Estrutura do repositório

```
├── Apresentacao/          # Sobre o curso, suporte e perguntas frequentes
│   ├── apresentacao.md
│   ├── apresentacao.tex
│   └── apresentacao.html
├── Licao01/                # Primeiros Passos
│   ├── licao01.md
│   ├── licao01.tex
│   └── licao01.html
├── Licao02/ ... Licao07/   # Demais lições (mesma estrutura)
└── Resolucoes/             # Resoluções dos exercícios de todas as lições
    ├── resolucoes01.md
    ├── resolucoes01.tex
    ├── resolucoes01.html
    └── ... (resolucoes02 a resolucoes07)
```

Cada lição e cada arquivo de resoluções segue o mesmo padrão de três formatos:

| Formato | Uso recomendado |
|---|---|
| `.md`   | Leitura direta no GitHub / editores de texto |
| `.tex`  | Compilação em PDF via LaTeX (ex.: ABNT, PDF formatado para impressão) |
| `.html` | Visualização direta no navegador — basta abrir o arquivo, sem necessidade de servidor |

## 📚 Conteúdo das lições

| Lição | Tópico | Link |
|---|---|---|
| Apresentação | Sobre o curso, suporte e FAQ | [apresentacao.md](Apresentacao/apresentacao.md) |
| Lição 1 | Primeiros Passos | [licao01.md](Licao01/licao01.md) |
| Lição 2 | Repetição e Métodos | [licao02.md](Licao02/licao02.md) |
| Lição 3 | Listas, Arrays e Strings | [licao03.md](Licao03/licao03.md) |
| Lição 4 | Orientação a Objetos — Fundamentos | [licao04.md](Licao04/licao04.md) |
| Lição 5 | Orientação a Objetos — Aprofundamento | [licao05.md](Licao05/licao05.md) |
| Lição 6 | Listas de Objetos, Testes e Programas Complexos | [licao06.md](Licao06/licao06.md) |
| Lição 7 | Paradigmas de Programação, Algoritmos e Exercícios Finais | [licao07.md](Licao07/licao07.md) |

> Cada lição inclui objetivos de aprendizagem, explicações teóricas e exemplos de código em Java.

## ✅ Resoluções

A pasta [`Resolucoes/`](Resolucoes) contém a resolução comentada de todos os exercícios propostos em cada lição correspondente (`resolucoesXX` refere-se aos exercícios de `licaoXX`), com código-fonte completo em Java e explicações do raciocínio utilizado.

| Lição | Resolução |
|---|---|
| Lição 1 | [resolucoes01.md](Resolucoes/resolucoes01.md) |
| Lição 2 | [resolucoes02.md](Resolucoes/resolucoes02.md) |
| Lição 3 | [resolucoes03.md](Resolucoes/resolucoes03.md) |
| Lição 4 | [resolucoes04.md](Resolucoes/resolucoes04.md) |
| Lição 5 | [resolucoes05.md](Resolucoes/resolucoes05.md) |
| Lição 6 | [resolucoes06.md](Resolucoes/resolucoes06.md) |
| Lição 7 | [resolucoes07.md](Resolucoes/resolucoes07.md) |

## 🚀 Como usar

1. Comece pela [Apresentação](Apresentacao/apresentacao.md) para entender o contexto do curso.
2. Siga as lições em ordem (`Licao01` → `Licao07`), lendo a teoria e tentando os exercícios propostos.
3. Depois de tentar os exercícios por conta própria, consulte a resolução correspondente na pasta `Resolucoes/` para comparar sua solução.
4. Prefira o `.md` para leitura rápida no GitHub, o `.html` para uma versão estilizada no navegador, e o `.tex` caso queira gerar um PDF formatado.

## 🛠️ Como compilar os arquivos LaTeX

Para gerar os PDFs a partir dos arquivos `.tex`:

```bash
pdflatex nome-do-arquivo.tex
```

> Recomenda-se compilar duas vezes para garantir a geração correta do sumário e referências cruzadas.

## 🌐 Fonte original e créditos

Conteúdo original em inglês disponível em: [java-programming.mooc.fi](https://java-programming.mooc.fi/), desenvolvido pelo grupo de pesquisa [Agile Education Research](https://www.helsinki.fi/en/researchgroups/data-driven-education) da Universidade de Helsinque.

A tradução e adaptação para o português presentes neste repositório foram produzidas com apoio de ferramentas de IA, com revisão dos alunos responsáveis pelo projeto de extensão.

---

*Projeto de extensão desenvolvido por alunos do curso de Engenharia de Computação da Universidade Federal de Uberlândia (UFU), como parte dos estudos do curso Java Programming I.*
