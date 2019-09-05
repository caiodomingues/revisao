# Revisão

1. [Revisão](#Revisão)
2. [Sobrecarga](#Sobrecarga)
3. [Sobrescrita](#Sobrescrita)
4. [Abstração](#Abstração)
5. [Interface](#Interface)
6. [Casting](#Casting)

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

Na sobreposição é **necessário que os métodos tenham a mesma assinatura**, mas com **implementações diferentes.**

## Sobrescrita

O Polimorfismo de Sobrescrita (override), algumas vezes descrito como Sobreposição (override) é a permissão de reescrever um método em uma subclasse que possua um comportamente diferente do método de mesma assinatura na superclasse.

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

***OBS:**Apesar de não utilizarmos o override aqui, a sobrescrita vai ocorrer, já que estamos tratando uma classe abstrata, entretanto o compilador retornará uma mensagem de aviso relativo a sobrescrita forçada pelo nome, o que não é uma boa prática. Utilizem a abstração ou virtualização em casos reais, saliento que este foi apenas um dos exemplos.*

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

    // Não há implementação / escopo
    public abstract void MetodoAbstrato(string nome); 
    
    // Há implemetação / escopo
    public virtual void MetodoVirtual(int x) {
        return x;
    }
}

// Modifica o método virtual
public class Derivada : Base {
    // Override obrigatório, método abstrato
    public override void MetodoAbstrato(string nome) {
        return nome;
    }
    
    // Override opcional, método virtual
    public override void MetodoVirtual(int x) {
        return x*2;
    }
}

// Não modifica o método virtual
public class OutraDerivada : Base {
    // Override obrigatório, método abstrato
    public override void MetodoAbstrato(string nome) {
        return nome;
    }
    
    // Sem override na virtual nesta classe
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

## Casting

A definição da Microsoft:

> Como o C# é tipado estaticamente no tempo de compilação, depois que uma variável é declarada, ela não pode ser declarada novamente ou atribuída a um valor de outro tipo, a menos que esse tipo possa ser convertido implicitamente no tipo da variável. Por exemplo, a string não pode ser convertida implicitamente em int. Portanto, depois de declarar i como um int, não é possível atribuir a cadeia de caracteres "Hello" a ele.
> No entanto, às vezes é necessário copiar um valor para uma variável ou um parâmetro de método de outro tipo. Por exemplo, você pode ter que passar uma variável de inteiro para um método cujo parâmetro é digitado como double. Ou talvez precise atribuir uma variável de classe a uma variável de um tipo de interface. Esses tipos de operações são chamados de conversões de tipo.

## General Casting

Não vou estender a explicação demais para não confudir com o conteúdo de prova, mas casting basicamente é:

```cs
public string numeroStr = "3";              // String "3"
public int numeroInt = (int) numeroStr;     // Int 3
```

## Upcasting e Downcasting

Upcasting converte um objeto de um tipo específico para outro geral. Downcasting faz o inverso. Exemplo:

```cs
BankAccount    ba1, ba2 =   new BankAccount("John", 250.0M, 0.01);
LotteryAccount la1, la2 =   new LotteryAccount("Bent", 100.0M);

ba1 = la2;                      // upcasting   - OK
//  la1 = ba2;                  // downcasting - Illegal 
                                //   discovered at compile time
//  la1 = (LotteryAccount)ba2;  // downcasting - Illegal
                                //   discovered at run time
la1 = (LotteryAccount)ba1;      // downcasting - OK 
                                //   ba1 already refers to a LotteryAccount

```
