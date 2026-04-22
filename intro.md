
# Aula 01 — Fundamentos da Linguagem Java e a JVM
---

## 🎯 Objetivos de Aprendizagem

Ao final desta aula, você será capaz de:

- Explicar a origem do Java e o problema de **portabilidade** que ele foi criado para resolver
- Distinguir os processos de **compilação** e **interpretação** e posicionar o Java entre eles
- Descrever o papel da **JVM** e o conceito de *"Write Once, Run Anywhere"*
- Diferenciar os componentes **JDK**, **JRE** e **JVM** e saber qual instalar para cada situação
- Reconhecer os tipos de arquivo Java (`.java`, `.class`, `.jar`) e seu papel no ciclo de desenvolvimento
- Escrever, compilar e executar o primeiro programa Java via terminal e via IDE
- Interpretar cada parte da assinatura obrigatória do **método `main()`**
- Usar `System.out.print()`, `System.out.println()` e `System.out.printf()` para formatar a saída
- Ler dados do teclado com a classe **`Scanner`**

---

## 1. O Problema que o Java Veio Resolver

Para entender por que o Java foi criado, é preciso entender o problema que existia antes dele.

Em 1991, linguagens como C e C++ dominavam o mercado. Elas geravam programas extremamente rápidos — mas com um custo alto: o código compilado era **atrelado a um sistema operacional e a um hardware específicos**.

```
Código C compilado no Windows  →  roda no Windows ✅
                                →  roda no Linux   ❌  (precisa recompilar)
                                →  roda no macOS   ❌  (precisa recompilar)
```

Para empresas que precisavam distribuir software para múltiplas plataformas — e para a Web emergente de 1993, onde qualquer pessoa com qualquer sistema poderia acessar um site — essa limitação era um obstáculo real.

A Sun Microsystems precisava de uma linguagem que **funcionasse em qualquer lugar sem modificações**. Esse foi o problema que motivou a criação do Java.

---

## 2. Uma Breve História

| Ano | Evento |
|---|---|
| **1991** | Sun Microsystems financia o projeto interno **Green**, liderado por **James Gosling**, com foco em dispositivos eletrônicos inteligentes. A linguagem inicial se chama **Oak** |
| **1993** | A *World Wide Web* explode em popularidade nos EUA. O projeto Oak é redirecionado para a Web |
| **Nome Java** | Como já existia uma linguagem chamada Oak, o nome é trocado para **Java** — homenagem a um café importado que a equipe tomava em uma cafeteria próxima à empresa |
| **23/05/1995** | A Sun apresenta oficialmente o Java na conferência **SunWorld**, junto com o mascote **Duke** |
| **2010** | A Oracle adquire a Sun Microsystems e passa a manter o Java |
| **Hoje** | Java é uma das linguagens mais usadas do mundo — presente em aplicações corporativas, sistemas Android e servidores de grande escala |

> 💡 **Dica:** O mascote Duke foi criado pelo designer Joe Palrang. Você o verá frequentemente em materiais oficiais da Oracle. O nome "Java" também inspirou o ícone da linguagem: uma xícara de café fumegante ☕.

---

## 3. Compilação vs. Interpretação

Para entender o que a JVM faz, precisamos antes entender as duas formas clássicas de executar um programa.

### 3.1 — Compilação

O código-fonte é traduzido **integralmente** para linguagem de máquina **antes** de ser executado. O resultado é um arquivo executável específico para uma plataforma:

```
Arquivo fonte (Teste.c)  →  Compilador  →  Teste.exe (Windows)
                                        →  Teste     (Linux — binário diferente)
```

| Vantagem | Desvantagem |
|---|---|
| Execução muito rápida | O executável não funciona em outro SO sem recompilar |
| Erros detectados antes de rodar | Ciclo editar–compilar–executar mais longo |

### 3.2 — Interpretação

O código-fonte é lido e executado **instrução por instrução**, em tempo real, por um programa chamado interpretador:

```
Programa Fonte  ─┐
                 ├──►  Interpretador  ──►  Saída
Dados           ─┘
```

| Vantagem | Desvantagem |
|---|---|
| Portabilidade: o mesmo fonte roda em qualquer lugar que tenha o interpretador | Execução mais lenta (traduz em tempo real) |
| Erros reportados durante a execução | Erros só aparecem quando aquela linha é executada |

### 3.3 — Java: a Abordagem Híbrida

Java combina o melhor dos dois mundos:

```
Código-fonte (.java)  →  javac (compila UMA vez)  →  Bytecode (.class)
                                                           │
                                           ┌───────────────┼───────────────┐
                                           ▼               ▼               ▼
                                       JVM Windows     JVM Linux       JVM macOS
                                       (interpreta)    (interpreta)    (interpreta)
```

- **Compile uma vez** (para Bytecode — independente de plataforma)
- **Execute em qualquer lugar** que tenha uma JVM instalada

> 💡 **Dica:** O Bytecode (arquivo `.class`) não é código de máquina para nenhum processador real. É um código intermediário projetado especificamente para ser lido pela JVM. Pense nele como um "idioma neutro" que toda JVM do planeta sabe traduzir.

---

## 4. A Máquina Virtual Java — JVM

A **JVM** (Java Virtual Machine) é o programa que lê o Bytecode e o executa. Ela cria uma camada de abstração entre o programa Java e o sistema operacional real:

```
┌─────────────────────────────────────────────┐
│             Aplicação Java (.class)          │
├─────────────────────────────────────────────┤
│       Máquina Virtual Java (JVM)             │  ← mesma interface para a aplicação
├──────────────┬──────────────┬────────────────┤
│   Windows    │    Linux     │     macOS      │  ← SOs diferentes por baixo
└──────────────┴──────────────┴────────────────┘
```

Além da execução, a JVM oferece serviços essenciais:

| Serviço | O que faz |
|---|---|
| **Verificação de Bytecode** | Confere a segurança do código antes de executar |
| **Garbage Collector (GC)** | Libera automaticamente a memória de objetos não utilizados — você não precisa gerenciar memória manualmente |
| **JIT Compiler** | *Just-In-Time*: partes do código executadas frequentemente são compiladas para código nativo em tempo real, acelerando a execução |
| **Tratamento de Exceções** | Gerencia erros em tempo de execução de forma padronizada |

> ⚠️ **Aviso de Professora:** A JVM é o coração do Java. Ao longo do semestre, conceitos como `NullPointerException`, Garbage Collector e gerenciamento de memória farão muito mais sentido quando você lembrar que há uma máquina virtual gerenciando tudo isso por baixo do seu código.

---

## 5. JVM, JRE e JDK — O Que Instalar?

Três siglas que causam confusão no início. Entenda a relação entre elas:

```
┌─────────────────────────────────────────────────────────┐
│                        JDK                              │
│  (Java Development Kit — Kit do Desenvolvedor)          │
│                                                         │
│   ┌───────────────────────────────────────────────┐     │
│   │                    JRE                        │     │
│   │  (Java Runtime Environment — Para o usuário) │     │
│   │                                               │     │
│   │   ┌───────────────────────────────────────┐   │     │
│   │   │               JVM                    │   │     │
│   │   │  (Java Virtual Machine — O motor)    │   │     │
│   │   └───────────────────────────────────────┘   │     │
│   │   + Bibliotecas padrão (java.util, java.io…)  │     │
│   └───────────────────────────────────────────────┘     │
│   + Compilador javac                                    │
│   + Ferramentas: javadoc, jar, jdb (depurador)          │
└─────────────────────────────────────────────────────────┘
```

| Componente | Para quem | O que contém |
|---|---|---|
| **JVM** | O motor interno | Interpreta o Bytecode; não se instala isoladamente |
| **JRE** | Usuário final | JVM + bibliotecas padrão — apenas para *executar* programas |
| **JDK** | Desenvolvedor | JRE + compilador `javac` + ferramentas de desenvolvimento |

> 💡 **Dica:** Como você está aqui para *desenvolver*, instale sempre o **JDK**. Ele já inclui tudo que você precisa — incluindo o JRE para executar seus próprios programas.

---

## 6. Edições da Plataforma Java

Java não é uma linguagem única — é uma plataforma com edições voltadas para diferentes contextos:

| Edição | Sigla | Para quê |
|---|---|---|
| Java Standard Edition | **JSE** | Aplicações desktop e de linha de comando — o que usamos nesta disciplina |
| Java Enterprise Edition | **JEE / Jakarta EE** | Aplicações corporativas em servidores — sistemas bancários, e-commerces |
| Java Micro Edition | **JME** | Dispositivos com recursos limitados: celulares antigos, PDAs, embarcados |
| Java Card | — | Cartões inteligentes com recursos extremamente limitados |

---

## 7. Tipos de Arquivo Java

Ao longo do semestre você trabalhará com estes arquivos:

| Extensão | Nome | Descrição |
|---|---|---|
| `.java` | Arquivo-fonte | O código que você escreve. Texto puro — editável em qualquer editor |
| `.class` | Bytecode | Gerado pelo compilador `javac`. Não é legível por humanos, mas qualquer JVM o executa |
| `.jar` | Java Archive | Empacotamento de múltiplos `.class` para distribuição (padrão ZIP) |

```
Você escreve →  HelloWorld.java
javac compila → HelloWorld.class   (Bytecode)
java executa  → HelloWorld.class   (a JVM o interpreta)
```

---

## 8. O Ambiente de Desenvolvimento

Para escrever e executar código Java, você precisa de duas coisas: o JDK e uma ferramenta de edição.

### 8.1 — Instalação do JDK

Instale o **JDK 17 ou superior** (versão LTS — Long Term Support):

| Distribuição | Link | Observação |
|---|---|---|
| Adoptium Temurin | [adoptium.net](https://adoptium.net/) | Recomendada para iniciantes — instalador simples |
| Oracle JDK | [oracle.com/java](https://www.oracle.com/java/technologies/downloads/) | Versão oficial da Oracle |

Verifique a instalação no terminal:

```bash
java -version
# Saída esperada: openjdk version "17.x.x" ...

javac -version
# Saída esperada: javac 17.x.x
```

> ⚠️ **Aviso de Professora:** Se `javac -version` retornar erro mas `java -version` funcionar, você instalou apenas o JRE, não o JDK. Volte e instale o JDK completo.

### 8.2 — IDEs Recomendadas

| IDE | Link | Quando usar |
|---|---|---|
| **IntelliJ IDEA Community** | [jetbrains.com/idea](https://www.jetbrains.com/idea/) | **Recomendada** — a mais usada na indústria |
| **VS Code** + Extension Pack for Java | [Tutorial VSCode](https://code.visualstudio.com/docs/languages/java) | Leve; boa para quem já usa o editor |
| **Eclipse IDE** | [eclipse.org](https://www.eclipse.org/downloads/) | Clássico; muito material de apoio disponível |
| **NetBeans** | [netbeans.apache.org](https://netbeans.apache.org/) | Simples; boa integração com o JDK |

---

## 9. O Primeiro Programa — Hello World

### 9.1 — Regras Fundamentais

Antes de escrever código, memorize três regras que valem para **todos** os programas Java:

1. **Todo código Java vive dentro de uma classe** — não existe instrução fora de uma classe
2. O **nome do arquivo `.java`** deve ser **idêntico** ao nome da classe pública (incluindo maiúsculas e minúsculas)
3. Java é **case-sensitive**: `HelloWorld` ≠ `helloworld` ≠ `HELLOWORLD`

### 9.2 — O Código

```java
// Arquivo: HelloWorld.java
// O nome do arquivo DEVE ser igual ao nome da classe pública

public class HelloWorld {

    // O método main() é o ponto de entrada do programa
    // A JVM procura por esse método exato para iniciar a execução
    public static void main(String[] args) {

        // System.out.println imprime o texto e vai para a próxima linha
        System.out.println("Olá, Mundo! Bem-vindos ao curso de Java 2026.");
    }
}
```

### 9.3 — Compilando e Executando no Terminal

```bash
# 1. Acesse a pasta onde o arquivo está
cd C:\caminho\para\a\pasta

# 2. Compile: gera HelloWorld.class
javac HelloWorld.java

# 3. Execute: a JVM carrega o .class e roda o main()
java HelloWorld
```

**Saída:**
```
Olá, Mundo! Bem-vindos ao curso de Java 2026.
```

---

## 10. Anatomia do Método `main()`

A assinatura do `main()` tem uma forma **exata e obrigatória**. A JVM procura por ela ao carregar uma classe — qualquer desvio e o programa não executa:

```java
public static void main(String[] args)
```

Cada palavra tem uma razão de existir:

| Palavra | O que significa | Por quê é obrigatória |
|---|---|---|
| `public` | Visível de qualquer lugar | A JVM precisa chamar este método de fora da classe |
| `static` | Pertence à classe, não a um objeto | A JVM não cria um objeto para chamar o `main` — ela o chama diretamente na classe |
| `void` | Não retorna valor | A JVM não espera um retorno deste método |
| `main` | Nome convencional do ponto de entrada | A JVM procura por este nome específico |
| `String[] args` | Array de textos passados pela linha de comando | Permite receber argumentos ao executar: `java HelloWorld argumento1` |

O parâmetro `args` pode ser declarado de três formas equivalentes:

```java
public static void main(String[] args)        // forma mais comum
public static void main(String args[])        // também válida
public static void main(String[] qualquerNome) // o nome não importa, o tipo sim
```

> ⚠️ **Aviso de Professora:** Qualquer alteração na assinatura do `main()` — como remover `static`, trocar `void` por `int` ou escrever `Main` em vez de `main` — faz com que a JVM não reconheça o método como ponto de entrada. O programa compila normalmente, mas ao executar surge o erro: `"Main method not found in class"`. Memorize a assinatura correta desde agora.

---

## 11. Saída de Dados — `System.out`

Java oferece três formas principais de imprimir dados no console:

| Método | Comportamento |
|---|---|
| `System.out.print(x)` | Imprime `x` e **mantém o cursor na mesma linha** |
| `System.out.println(x)` | Imprime `x` e **move o cursor para a próxima linha** |
| `System.out.printf("formato", valores)` | Imprime com **formatação precisa** (casas decimais, alinhamento) |

### 11.1 — `print` vs. `println`

```java
public class ExemploSaida {
    public static void main(String[] args) {
        System.out.print("Desenvolvimento");   // sem quebra
        System.out.print(" de");               // continua na mesma linha
        System.out.println(" Software");       // imprime e quebra a linha

        System.out.println("2026");            // já na linha seguinte
        System.out.println();                  // linha em branco
        System.out.println("===========");
    }
}
```

**Saída:**
```
Desenvolvimento de Software
2026

===========
```

### 11.2 — Caracteres de Escape

Dentro de uma `String`, certas sequências com `\` representam ações especiais:

| Escape | Nome | Efeito |
|---|---|---|
| `\n` | Nova linha | Cursor vai para o início da **próxima linha** |
| `\t` | Tabulação | Cursor avança até a próxima **parada de tabulação** |
| `\r` | Retorno de carro | Cursor volta ao **início da linha atual** |
| `\\` | Barra invertida | Imprime o caractere `\` literalmente |
| `\"` | Aspas duplas | Imprime `"` dentro de uma String |

```java
System.out.println("Linha 1\nLinha 2");         // quebra de linha no meio da String
System.out.println("Nome:\tJuliana");            // tabulação
System.out.println("Ela disse: \"Olá!\"");      // aspas dentro da String
System.out.println("Caminho: C:\\Documentos");  // barra invertida em path Windows
```

**Saída:**
```
Linha 1
Linha 2
Nome:	Juliana
Ela disse: "Olá!"
Caminho: C:\Documentos
```

### 11.3 — `printf` para Formatação Precisa

O `printf` usa **especificadores de formato** para controlar exatamente como cada valor é exibido:

| Especificador | Tipo | Exemplo |
|---|---|---|
| `%d` | Inteiro | `printf("%d", 42)` → `42` |
| `%f` | Ponto flutuante | `printf("%.2f", 3.14159)` → `3,14` |
| `%s` | String | `printf("%s", "Java")` → `Java` |
| `%n` | Quebra de linha | Equivale a `\n`, mas portável entre SOs |
| `%10d` | Inteiro com 10 chars de largura (alinhado à direita) | `printf("%10d", 42)` → `        42` |

```java
String nome   = "Ana Lima";
int    numero = 1001;
double saldo  = 1234.56;

System.out.printf("Titular : %s%n",     nome);
System.out.printf("Conta   : %d%n",     numero);
System.out.printf("Saldo   : R$ %.2f%n", saldo);
```

**Saída:**
```
Titular : Ana Lima
Conta   : 1001
Saldo   : R$ 1234,56
```

> 💡 **Dica:** Prefira `printf` ao invés de concatenação com `+` quando precisar formatar números. `"Saldo: R$" + saldo` pode exibir `1234.5599999` dependendo do tipo, enquanto `printf("R$ %.2f", saldo)` sempre exibe exatamente 2 casas decimais.

---

## 12. Leitura de Dados — Classe `Scanner`

Para ler dados digitados pelo usuário no teclado, usamos a classe `Scanner`, que precisa ser **importada** antes de ser usada:

```java
import java.util.Scanner; // importação obrigatória — deve vir antes da classe

public class ExemploScanner {
    public static void main(String[] args) {

        // Cria um Scanner conectado ao teclado (System.in)
        Scanner input = new Scanner(System.in);

        System.out.print("Digite seu nome: ");
        String nome = input.nextLine();   // lê uma linha inteira de texto

        System.out.print("Digite sua idade: ");
        int idade = input.nextInt();      // lê um número inteiro

        System.out.printf("Olá, %s! Você tem %d anos.%n", nome, idade);

        input.close(); // boa prática: libera o recurso ao terminar
    }
}
```

**Exemplo de execução:**
```
Digite seu nome: Juliana
Digite sua idade: 30
Olá, Juliana! Você tem 30 anos.
```

**Principais métodos de leitura:**

| Método | Tipo lido | Observação |
|---|---|---|
| `nextInt()` | `int` | Lê um inteiro |
| `nextDouble()` | `double` | Lê um decimal |
| `nextLong()` | `long` | Lê um inteiro longo |
| `next()` | `String` | Lê **uma palavra** (para no espaço) |
| `nextLine()` | `String` | Lê a **linha inteira** (incluindo espaços) |
| `nextBoolean()` | `boolean` | Lê `true` ou `false` |

> ⚠️ **Aviso de Professora:** Um erro comum: chamar `nextLine()` logo após `nextInt()`. O `nextInt()` lê o número mas **deixa a quebra de linha `\n` no buffer**. O `nextLine()` seguinte lê essa quebra vazia e parece ter "pulado" a leitura. A solução é adicionar um `input.nextLine()` descartável entre os dois:
>
> ```java
> int idade = input.nextInt();
> input.nextLine(); // descarta o '\n' residual
> String nome = input.nextLine(); // agora lê corretamente
> ```

---

## 13. Programa Completo — Juntando Tudo

```java
import java.util.Scanner;

/**
 * Demonstração dos conceitos desta aula: output formatado,
 * leitura de dados e estrutura básica de um programa Java.
 *
 * @author Juliana Costa-Silva
 */
public class CadastroSimples {

    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        System.out.println("╔══════════════════════════════════╗");
        System.out.println("║     CADASTRO DE ALUNO — 2026     ║");
        System.out.println("╚══════════════════════════════════╝\n");

        // Leitura de dados do usuário
        System.out.print("Nome completo  : ");
        String nome = input.nextLine();

        System.out.print("Número de matrícula: ");
        int matricula = input.nextInt();

        System.out.print("Nota 1: ");
        double nota1 = input.nextDouble();

        System.out.print("Nota 2: ");
        double nota2 = input.nextDouble();

        // Processamento
        double media = (nota1 + nota2) / 2.0;
        String situacao = (media >= 6.0) ? "Aprovado ✅" : "Reprovado ❌";

        // Exibição formatada do resultado
        System.out.println("\n── Resultado ──────────────────────────");
        System.out.printf("Aluno     : %s%n",    nome);
        System.out.printf("Matrícula : %d%n",    matricula);
        System.out.printf("Nota 1    : %.1f%n",  nota1);
        System.out.printf("Nota 2    : %.1f%n",  nota2);
        System.out.printf("Média     : %.1f%n",  media);
        System.out.printf("Situação  : %s%n",    situacao);

        input.close();
    }
}
```

**Exemplo de execução:**
```
╔══════════════════════════════════╗
║     CADASTRO DE ALUNO — 2026     ║
╚══════════════════════════════════╝

Nome completo  : Carlos Neto
Número de matrícula: 20260042
Nota 1: 7.5
Nota 2: 6.0

── Resultado ──────────────────────────
Aluno     : Carlos Neto
Matrícula : 20260042
Nota 1    : 7,5
Nota 2    : 6,0
Média     : 6,8
Situação  : Aprovado ✅
```

---

## 14. Onde Esta Aula se Encaixa no Semestre

Esta aula fornece a **infraestrutura de linguagem** que todas as aulas seguintes usarão. Veja o que vem pela frente:

```
Aula 01 — Você está aqui ▼
│   JVM, Hello World, main(), print, Scanner
│   "Sei escrever e executar código Java"
│
▼ Aulas 02–03: Fundamentos
│   Tipos primitivos, operadores, if/else, switch, loops
│   "Sei controlar o fluxo do programa"
│
▼ Aula 04: Introdução à POO
│   Classes, objetos, atributos, métodos, new
│   "Sei criar minhas próprias entidades"
│
▼ Aula 05: Encapsulamento
│   private, public, getters, setters
│   "Sei proteger o estado dos meus objetos"
│
▼ Aula 06: Construtores e Herança
│   this(), super(), extends, @Override
│   "Sei construir hierarquias reutilizáveis"
│
▼ Aulas seguintes: Polimorfismo, Abstração, Interfaces
    "Sei projetar sistemas flexíveis e extensíveis"
```

> 💡 **Observe:** nesta primeira aula, todo o código vive dentro do `main()`. Isso é proposital — queremos que você se familiarize com a sintaxe antes de pensar em classes e objetos. A partir da **Aula 04**, você entenderá por que escrever tudo no `main` não escala — e aprenderá a organizar o código de forma orientada a objetos.

---

## 🛠️ Atividades Práticas

### Atividade 1 — Configuração e Verificação do Ambiente

Execute os comandos abaixo no terminal e registre as saídas:

```bash
java -version
javac -version
```

Responda:
- Qual versão do Java foi exibida?
- O comando `javac` foi reconhecido? Se não, o que isso indica sobre sua instalação?
- Qual a diferença entre executar `java` e `javac`?

---

### Atividade 2 — Hello, Você!

Crie um arquivo `OlaNome.java` e modifique o Hello World da aula para:

**Requisitos:**
- Exibir seu nome completo
- Exibir o nome do seu curso
- Usar `\n` dentro de uma única chamada `println` para formatar a saída em duas linhas
- Usar `\t` para adicionar um recuo antes do nome do curso

**Saída esperada (adapte com seus dados):**
```
Bem-vindo ao curso de Java 2026!
	Aluno  : Seu Nome Aqui
	Curso  : Análise e Desenvolvimento de Sistemas
```

Compile e execute via terminal: `javac OlaNome.java` → `java OlaNome`.

---

### Atividade 3 — Calculadora Interativa

Crie um programa `Calculadora.java` que leia dois números reais do usuário e exiba os resultados das quatro operações:

**Requisitos:**
- Use `Scanner` para ler os dois valores
- Exiba adição, subtração, multiplicação e divisão
- Formate todos os resultados com exatamente **2 casas decimais** usando `printf`
- Trate o caso em que o segundo número é zero: exiba `"Divisão por zero — operação inválida"` nesse caso (use `if`)

**Saída esperada para entradas 10.0 e 3.0:**
```
Digite o primeiro número : 10.0
Digite o segundo número  : 3.0

── Resultados ─────────────────────
10,00 + 3,00  =  13,00
10,00 - 3,00  =   7,00
10,00 × 3,00  =  30,00
10,00 ÷ 3,00  =   3,33
```

---

### Atividade 4 — Desafio: Ficha de Cadastro Completa

Crie um programa `FichaCadastro.java` que colete e exiba um cadastro completo:

**Dados a coletar via `Scanner`:**
- Nome completo (String — linha inteira)
- Matrícula (int)
- Três notas bimestrais (double cada)
- Percentual de presença (double — ex: 87.5)

**Processamento:**
- Calcular a **média das três notas**
- Determinar a **situação**: `"Aprovado"` (média ≥ 6.0), `"Recuperação"` (média ≥ 4.0 e < 6.0) ou `"Reprovado"` (média < 4.0)
- Verificar a **frequência**: aprovado por frequência se presença ≥ 75%; reprovado por falta caso contrário
- A situação final deve considerar **ambos** os critérios

**Exibição:**
- Use `printf` para alinhar todos os campos
- Exiba uma borda com `╔`, `║`, `╚` ao redor do resultado (veja o `CadastroSimples` desta aula como modelo)
- Exiba a situação final com um emoji indicativo (✅ para aprovado, ⚠️ para recuperação, ❌ para reprovado)

---

## 📚 Referências e Leituras Recomendadas

| Recurso | Capítulos | Relevância |
|---|---|---|
| Deitel & Deitel — *Java: Como Programar* (10ª ed.) | Cap. 1 (Introdução aos Computadores, Internet e Java) e Cap. 2 (Hello World) | ⭐⭐⭐ Essencial |
| Horstmann & Cornell — *Core Java* Vol. I | Cap. 1 (An Introduction to Java) e Cap. 2 (The Java Programming Environment) | ⭐⭐⭐ Essencial |
| Silveira & Amaral — *Java SE 8 Programmer I* | Cap. 1 (Fundamentos da plataforma Java) | ⭐⭐ Recomendado |
| Oracle — [The Java™ Tutorials: Getting Started](https://docs.oracle.com/javase/tutorial/getStarted/) | — | ⭐⭐ Recomendado |
| Oracle — [Java SE Documentation](https://docs.oracle.com/en/java/javase/17/) | — | ⭐ Referência |

---

## 🚀 Próximos Passos

- Explorar os **tipos primitivos** do Java (`int`, `double`, `boolean`, `char`) e as regras de nomenclatura de variáveis — temas das próximas aulas
- Praticar a compilação pelo terminal até que o fluxo `editar → javac → java` seja automático
- Configurar sua IDE preferida e reproduzir os exemplos desta aula dentro dela

---
[⬅️ Voltar ao Início](README.md)
