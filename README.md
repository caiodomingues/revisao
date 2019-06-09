# Revisão - POO

1. [Revisão](#Revisão)
2. [Sobrecarga](#Sobrecarga)
3. [Sobrescrita](#Sobrescrita)
4. [Abstração](#Abstração)
5. [Interface](#Interface)

## Revisão

Nesta seção, estão descritas revisões sobre os conceitos de Herança e Polimorfismo, essenciais para o entendimento do conteúdo da prova, caso necessário, [pule para a próxima seção](#Sobrecarga).

### Herança e Polimorfismo

Em linhas gerais, a herança define uma espécie de árvore hereditária, onde possuímos pai, seus filhos, filhos dos filhos etc. No POO, possuimos a classe pai ou superclasse, e as classes filhas ou subclasses. A superclasse costuma ser estruturada de forma abrangente a poder criar diferentes tipos de subclasses de acordo com sua necessidade.

Em um exemplo simples, defina `Caneta` como uma **superclasse**, `Cor = azul` como um atributo e `Escrever()` como um método. Desta classe, podemos agora criar uma classe filha derivada de `Caneta`, a `CanetaVermelha`, porém, vamos sobrescrever o atributo `Cor` para `Cor = vermelha`.

A herança permite a reutilização de código e anda lado a lado com o polimorfismo, que acabamos de utilizar no exemplo anterior, alterando valores definidas por uma classe pai na classe filha. Para o polimorfismo, iremos categorizar em dois tipos, o polimorfismo de [Sobrecarga](#Sobrecarga) e [Sobrescrita](#Sobrescrita).

## Sobrecarga

O Polimorfismo de Sobrecarga (overload) permite que, dentro da mesma classe, mais de um método possa conter o mesmo nome, entretanto, eles **obrigatoriamente** devem possuir argumentos diferentes para funcionar. Os métodos construtores são um exemplo de Sobrecarga:

```cs
public class Caneta {

    public Caneta(string cor, string material) {
        Cor = cor;
        Material = material;
    }

    public Caneta(string cor) {
        Cor = cor;
    }
}
```

Podemos sobrecarregar métodos da seguinte forma:

```cs
public class Calculadora{

    public int calcula(int a, int b){
        return a+b;
    }

    public double calcula(double a, double b){
        return a+b;
    }

    public string calcula(string a, string b){
        return a+b;
    }

}
```

O programa é capaz de identificar qual dos métodos será executado com base nos parâmetros utilizados na chamada do método.

Na sobreposição é **necessário que os métodos tenham a mesma assinatura** (tipo de retorno, nome do método, tipos e quantidades de parâmetros), mas com **implementações diferentes.**

## Sobrescrita

O Polimorfismo de Sobrescrita (override) é a permissão de reescrever um método em uma subclasse que possua um comportamente diferente do método de mesma assinatura na superclasse.

Simplificando: uma classe filha pode reescrever os métodos implementados previamente na classe pai, ou seja, **uma classe filha pode redefinir métodos herdados de suas descendentes, mantendo o nome e a assinatura.**

Vou utilizar a mesma classe Calculadora, do exemplo anterior.

*Note que a classe agora é **abstrata***.

```cs
public abstract class Calculadora{
    public int calcula(int a, int b) {
        return a+b;
    }
}
```

E em uma classe filha:

```cs
public class Cientifica : Calculadora {
    public int calcula(int a, int b) {
        if (a >= 0 && b >= 0) {
            System.out.println("Número positivos.");
        }
        return a*b;
    }
}
```

## Abstração

Como demonstrado anteriormente nos exemplos, as classes abstratas criam superclasses ou possibilitam a Sobrescrita de atributos e métodos. Devemos saber que:

### Classes Abstratas

- Uma classe abstrata é uma classe que não pode ser instanciada. Você não pode criar um objeto a partir de uma classe abstrata.
- Uma classe abstrata pode ser herdada e geralmente serve como classe base para outras classes.
- Uma classe abstrata pode conter métodos abstratos e métodos comuns. Uma classe abstrata também podem possuir construtores, propriedades, indexadores e eventos.
- Uma classe abstrata não pode ser estática (static). Uma classe abstrata não pode ser selada (sealed).
- Uma classe abstrata pode herdar de outra classe abstrata.

### Métodos Abstratos

- Um método abstrato é um método que não possui implementação na classe abstrata. Um método abstrato possui somente a definição de sua assinatura. A sua implementação deve ser feita na classe derivada.
- Um método abstrato é um método virtual e deve ser implementado usando o modificador override.
- Um método abstrato somente pode existir em uma classe abstrata.
- Um método abstrato não pode usar os modificadores static e virtual.

### Abstract, Virtual e Interfaces

Utilizaremos o exemplo de uma forma geométrica para aplicar os conceitos de abstract, virtual e interfaces, aplicaremos também conceitos prévios.

```cs
public abstract class Forma
{
    private double _area;
    private string _cor;
    private double _perimetro;

    public string Cor
    {
        get { return _cor; }
        set { _cor = value; }
    }

    public double Area
    {
        get { return _area; }
        set { _area = value; }
    }

    public double Perimetro
    {
        get { return _perimetro; }
        set { _perimetro = value; }
    }

    public abstract void CalcularArea();
    public abstract void CalcularPerimetro();
    public string Descricao()
    {
        return "Sou a classe abstrata Forma.";
    }
}

```

Para esta classe, dizemos que a mesma é abstrata, tornando-a uma classe pai / superclasse. Note que os métodos `CalcularArea()` e `CalcularPerimetro()` também estão definidas como abstratas. Agora, para a criação de uma classe filha / subclasse, agora **obrigatoriamente** temos de [sobrescrever](#Sobrescrita) estas funções nas classes filhas, exemplo:

```cs
public class Quadrado : Forma {
    private double lado;

    public double Lado
    {
        get { return lado; }
        set { lado = value; }
    }

    public override void CalcularArea()
    {
        this.Area = lado * lado;
    }

    public override void CalcularPerimetro()
    {
        this.Perimetro = 4 * lado;
    }
}
```

Utilizamos o modificar `override` para a implementação dos métodos abstratos.

---

Diferentemente do modificador `abstract` que **devem** ter uma implementação (override), os métodos `virtual` **podem** ser sobrepostos por uma classe derivada. Em um rápido exemplo:

```cs
public abstract class Base {
    public abstract void MetodoAbstrato(string nome); 
    // Não há implementação

    public virtual void MetodoVirtual(int x) {
        return x;
    }
}

// Modifica o método virtual
public class Derivada : Base {
    public override void MetodoAbstrato(string nome) {
        return nome;
    }
    public override void MetodoVirtual(int x) {
        return x*2;
    }
}

// Não modifica o método virtual
public class OutraDerivada : Base {
    public override void MetodoAbstrato(string nome) {
        return nome;
    }
}
```

## Interface

Uma interface, é um tipo de classe que contém apenas as assinaturas de métodos, propriedades, eventos e indexadores.
Interface é uma estrutura que permite ao desenvolvedor especificar todos os métodos e propriedades que ele deseja que as classes que a implementam disponibilizem.

Utilizando uma interface, é possível separar completamente a definição/assinatura de métodos de suas respectivas implementações, ou seja, de um lado (interface) temos a definição dos mesmos e do outro temos a implementação dos mesmos.

Podemos entender uma interface como sendo uma espécie de contrato, ou seja, se uma classe X implementa uma interface Y, logo, Y garante que X disponibilizará todos os métodos definidos em Y. Caso este “contrato” seja quebrado, um erro será gerado.

Interfaces são muito importantes pois, nos permitem separar o “o que” do “como”. A interface não se preocupa com a forma com a qual o método está sendo implementado e sim, que este método estará disponível a todos os objetos que a implementarem. A interface descreve a maneira como você deseja que o objeto seja utilizado ao invés da forma como ele foi implementado.

A implementação dos membros é feita por uma classe concreta.
Para definir uma interface, utilizamos o modificar `interface`:

```cs
interface ICaneta {
    void Escrever();
}
```

A implemetação deve ser feita por uma classe concreta:

```cs
public class Caneta : ICaneta {
    public void Escrever() {
        // código
    }
}
```

***OBS:** o exemplo acima define uma interface e uma aplicação simples, há diferentes formas de realizar interfaces.*

É importante ressaltar:

- Uma interface é como uma classe base abstrata que contém apenas membros abstratos. Qualquer classe ou struct que implementa a interface deve implementar todos os seus membros.
- Uma interface não pode ser instanciada diretamente. Seus membros são implementados por qualquer classe ou struct que implemente a interface.
- As interfaces podem conter propriedades, indexadores, métodos e eventos.
- As interfaces não têm implementações de métodos.
- Uma classe ou struct pode implementar várias interfaces. - Uma classe pode herdar uma classe base e também implementar uma ou mais interfaces.
