
# 📘 Padrões de Projeto – Resumo de Estudos

Material completo e organizado **com base no conteúdo passado em sala**.  
Serve como guia de consulta rápida para provas, listas de exercícios e projetos.

---

## 📅 29/jul – Associação entre Objetos e Arrays de Objetos

**Exemplo da aula:** `Autor`, `Livro`, `Acervo`.

### Conceitos-chave
- **Associação:** uma classe contém outra como atributo.
- **Array de objetos:** coleção de instâncias dentro de uma classe.
- **Boas práticas:** encapsulamento, construtores, métodos de exibição.

### Exemplo em Java
```java
class Autor {
    private String nome, nacionalidade;

    public Autor(String nome, String nacionalidade) {
        this.nome = nome;
        this.nacionalidade = nacionalidade;
    }

    public String getNome() { return nome; }

    public void exibirDados() {
        System.out.println("Autor: " + nome + ", nacionalidade: " + nacionalidade);
    }
}

class Livro {
    private String titulo;
    private Autor autor; // associação

    public Livro(String titulo, Autor autor) {
        this.titulo = titulo;
        this.autor = autor;
    }

    public void exibirDados() {
        System.out.println("Livro: " + titulo + ", autor: " + autor.getNome());
    }
}

class Acervo {
    private Livro[] livros;

    public Acervo(int capacidade) {
        this.livros = new Livro[capacidade];
    }

    public void adicionarLivro(int posicao, Livro livro) {
        livros[posicao] = livro;
    }

    public void exibirLivros() {
        for (Livro l : livros)
            if (l != null) l.exibirDados();
    }
}
```

---

## 📅 05/ago – Herança e Polimorfismo

### Conceitos-chave
- **Herança:** `class Filha extends Mae`.
- **Polimorfismo:** variável de superclasse referenciando subclasse.
- **Sobrescrita:** `@Override` obrigatório.

### Exemplo em Java
```java
abstract class Veiculo {
    void exibir() { System.out.println("Veículo"); }
}

class Carro extends Veiculo {
    @Override void exibir() { System.out.println("Carro"); }
}

class Moto extends Veiculo {
    @Override void exibir() { System.out.println("Moto"); }
}

public class Main {
    public static void main(String[] args) {
        Veiculo v = new Carro(); // polimorfismo
        v.exibir();              // imprime "Carro"
    }
}
```

---

## 📅 12/ago – Interfaces e Tratamento de Erros

### Interfaces
```java
interface Forma {
    double calcularArea();
    void imprimir();
}

class Circulo implements Forma {
    private double raio;
    public Circulo(double raio) { this.raio = raio; }

    public double calcularArea() { return Math.PI * raio * raio; }
    public void imprimir() { System.out.println("Isso é um círculo."); }
}

class Retangulo implements Forma {
    private double largura, altura;
    public Retangulo(double largura, double altura) {
        this.largura = largura;
        this.altura = altura;
    }
    public double calcularArea() { return largura * altura; }
    public void imprimir() { System.out.println("Isso é um retângulo."); }
}
```

### Exceções
```java
try (BufferedReader bf = new BufferedReader(new FileReader("dados.txt"))) {
    String linha;
    while ((linha = bf.readLine()) != null) System.out.println(linha);
} catch (FileNotFoundException e) {
    e.printStackTrace();
} catch (IOException e) {
    e.printStackTrace();
}

try {
    int numero = Integer.parseInt("234r"); // erro proposital
} catch (NumberFormatException e) {
    e.printStackTrace();
}
```

---

## 📅 19/ago – Injeção de Dependências (DI)

### Sem interface
```java
class Motor {
    void ligar(){ System.out.println("Motor ligado!"); }
}

class Carro {
    private Motor motor;

    Carro(Motor motor){ this.motor = motor; }  // injeção
    void ligarCarro(){ motor.ligar(); }
}
```

### Com interface
```java
interface Motor { void ligar(); }

class MotorGasolina implements Motor {
    public void ligar(){ System.out.println("Motor a gasolina ligou!"); }
}

class MotorEletrico implements Motor {
    public void ligar(){ System.out.println("Motor elétrico ligou!"); }
}

class Carro {
    private Motor motor;
    Carro(Motor motor){ this.motor = motor; }
    void ligarCarro(){ motor.ligar(); }
}
```

---

## 📅 26/ago – Os 5 Princípios SOLID

### S – Single Responsibility
Cada classe tem uma única responsabilidade.

### O – Open/Closed
```java
interface Exportador { void exportar(); }

class PDFExportador implements Exportador {
    public void exportar(){ System.out.println("Exportando PDF..."); }
}

class CSVExportador implements Exportador {
    public void exportar(){ System.out.println("Exportando CSV..."); }
}

class RelatorioService {
    private Exportador exportador;
    RelatorioService(Exportador exportador){ this.exportador = exportador; }

    void gerarRelatorio(){
        System.out.println("Gerando relatório...");
        exportador.exportar();
    }
}
```

### L – Liskov Substitution
```java
abstract class Ave { public abstract void exibir(); }

interface AveQueVoa { void voar(); }

class Pardal extends Ave implements AveQueVoa {
    public void exibir(){ System.out.println("Sou um pardal!"); }
    public void voar(){ System.out.println("Pardal voando..."); }
}

class Pinguim extends Ave {
    public void exibir(){ System.out.println("Sou um pinguim e não voo!"); }
}
```

### I – Interface Segregation
Interfaces pequenas e específicas (`Exportador`, `AveQueVoa`).

### D – Dependency Inversion
Depender de **abstrações**, não de implementações concretas.

---

## 📅 02/set – Singleton

### Implementação básica
```java
public class Configuracao {
    private static Configuracao instancia; // única instância

    private Configuracao() { }

    public static synchronized Configuracao getInstancia() {
        if (instancia == null) instancia = new Configuracao();
        return instancia;
    }
}
```

### Double-checked locking
```java
public class Configuracao {
    private static volatile Configuracao instancia;
    private Configuracao(){}

    public static Configuracao getInstancia() {
        if (instancia == null) {
            synchronized (Configuracao.class) {
                if (instancia == null) instancia = new Configuracao();
            }
        }
        return instancia;
    }
}
```

---

# ✅ Checklist rápido (pra consulta em 2h)

- Arrays: inicializou e checou `null`?
- Associação: setou a composição antes de usar?
- Polimorfismo: variáveis do tipo **abstração** (`Forma`, `Exportador`, `Ave`)?
- Interfaces: implementou todos os métodos com `@Override`?
- Exceções: ordem do `catch` do mais específico pro genérico?
- DI: injeção por construtor, usando interface.
- SOLID:  
  - SRP: 1 classe = 1 responsabilidade  
  - OCP: aberto p/ extensão, fechado p/ modificação  
  - LSP: sem quebrar substituição (pinguim não voa)  
  - ISP: interfaces pequenas  
  - DIP: depende de abstrações  
- Singleton: construtor privado + método `getInstancia()`.

---

