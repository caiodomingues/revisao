# Revisão - ASP.NET CORE

1. [Introdução](#Introdução)
    1. [Model](#Model)
    2. [View](#View)
    3. [Controller](#Controller)
2. [ASP.NET](#ASP.NET)
3. [Estudo](#Estudo)

## Introdução

Para começar precisamos entender o que o padrão `MVC` significa.

- M = Model 
- V = View
- C = Controller

Este modelo foi criado para aplicar diversos conceitos de segurança, usabilidade de dados etc, bem como aplicar um dos principios mais famosos entre os programadores que é o `DRY` (Don't Repeat Yourself - Não se repita).

## Model

Os modelos são as classes da sua aplicação, definidas como visto pela matéria de Análise de Projeto Orientado a Objetos.

> *pausa para os risos*

<center>
![class-diagram](https://aec67204-a-62cb3a1a-s-sites.googlegroups.com/site/mindbitufrpe/diagramas/diagrama-de-classes/Class%20Diagram0.png)
</center>

Nestes modelos você precisa definir todos os atributos que você vai precisar em cada classe.

## View

View é o layout da aplicação, ou seja, o que o cliente vai "ver" na tela.

É possivel trazer uma variavel e/ou atributo criada do modelo para a view, isto significa que sempre que você trocar no seu modelo isto vai automaticamente trocar na sua view também, ou seja, uma aplicação dinâmica. O `Razor` é a View Engine responsável pelo tratamento das informações para a página, ele permite a inserção de código C# em conjunto com o HTML de uma view.

<center>
![razor render](https://docs.microsoft.com/en-us/xamarin/cross-platform/platform/razor-html-templates/images/image12_700x421.png)
</center>

Dentro da pasta View da sua aplicação haverá uma pasta `Shared`. A partir desta, podemos entender melhor o porque a introdução do conceito `DRY`.

A pasta `Shared` é responsável por manter todos os arquivos de layouts comuns de páginas, uma espécie de componentização de suas views. Exemplo:

Se um site possui uma navbar (menu) em todas as outras páginas, você não precisa reescrever o mesmo código várias vezes, bem como não precisa fazer alguma alteração em diversos arquivos simultâneamente, um único arquivo de layout pode conter este trecho de código da navbar, que é posteriormente importada pelas outras views, facilitando a manutenção e usabilidade do código. Por padrão, há um arquivo chamado `_Layout.cshtml` na pasta `Shared`. 

## Controller

Controllers são responsáveis por realizar as funções da página, ou seja, este controllers são os responsáveis por saber qual view pertence a qual modelo e para onde vai os dados recebidos pelo cliente ou pelo servidor. Isto é, ser responsável por tratar as requisições, sejam estas GET, POST etc.

<center>
![mvc](https://firebirdsql.org/file/documentation/reference_manuals/fbdevgd-en/html/images/fbdevgd30_mvc_001_en.png)
</center>

# ASP.NET

O ASP.NET é um Framework que extende o .NET para a web. Responsável por realizar um `MVC`:

- Separaçndo as tarefas da aplicação (Entrada lógica, lógica de negócio e a lógica de Interface);
- Por meio de um framework extensível e conectável. Os componentes são projetados para facilmente serem substituídos ou customizados. Você pode plugá-lo à sua política de roteamento de URL (URL Routing), conectá-lo a sua própria engine de visualização e outros componentes.
- Por meio de um componente poderoso de URL-mapping, que lhe permite criar aplicativo com URLs compreensíveis e que sejam de fácil localização por buscadores. Além de poder utilizar um padrão para nomeação de URLs, reforçando a ideia de localização otimizada (SEO - Search Engine Optimization).
- Suporte a recursos existentes do ASP.NET, pertitindo a utilização de recursos como autenticação de formulários e Windows Authentication, autorização URL (URL Authorization), data caching, gerenciamento de estado de sessão e perfil, o sistema de configuração e a arquitetura de provider.

# Estudo

## Data Annotations

<center>
![data annotations](http://keyurraval.com/blogimages/Asp_Net_MVC_DataAnnotation_Range_Attribute_1.png)
</center>

São utilizados para atribuir informações extras à variável, isto é, metadados. Podemos utilizá-los para definir:

- Chave primária usando `[Key]`;
- Nome de exibição da página usando `['Display(Name="exemplo")]`
- Validações, como um campo obrigatório usando `[Required]`, `[Required(ErrorMessage="exemplo de mensagem de erro")`;
- Uma expressão regular para filtrar o campo usando `[RegularExpression(".+\\@.+\\..+")`, também possui o ErrorMessage como opção;
- Tamanho de caracteres no campo usando `[StringLength(10,MinimumLength=4)]`;
- Tipo de dado usando `[DataType(DataType.Password)]`;
- Filtragem de display usando `[DisplayFormat(DataFormatString = "mm/dd/yyyy")`;
- Um intervalo usando `[Range(18,65)]`;

Entre outras que você pode conferir nas referências do ASP.NET;

## Program.cs e Startup.cs

O arquivo `Startup.cs` é o ponto de entrada do nível do aplicativo e lida com o pipeline de requisições, é acionada assim que o aplicativo é inicializado. Todo programa ASP.NET **DEVE**  ter o arquivo `startup.cs` (ao menos um, é possível ter mais de um).

Segundo à referência do ASP.NET:

> Os aplicativos do ASP.NET Core usam uma classe Startup, que é chamada de Startup por convenção. A classe Startup:
> - Opcionalmente, inclua um método ConfigureServices para configurar os serviços do aplicativo. Um serviço é um componente reutilizável que fornece a funcionalidade do aplicativo. Os serviços estão configurados—também descritos como registrados—em ConfigureServices e consumidos no aplicativo por meio da DI (injeção de dependência) ou ApplicationServices.
> - Inclui um método Configure para criar o pipeline de processamento de solicitações do aplicativo.
> - ConfigureServices e Configure são chamados pelo tempo de execução do ASP.NET Core quando o aplicativo é iniciado

Você pode ler mais sobre [aqui](https://docs.microsoft.com/pt-br/aspnet/core/fundamentals/startup?view=aspnetcore-2.2).

O arquivo `Program.cs` é o local onde podemos criar um host para o aplicativo da web:

```cs
public class Program {
    public static void Main(string[] args) {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<startup>()
            .Build();
}
</startup>
```

## Object Relational Mapper (ORM)

O ORM é uma técnica de mapeamento de objeto relacional que permite fazer uma relação dos objetos com os dados que os mesmos representam.
No nosso caso, utilizamos o Entity Framework ([Referência no MS Docs](https://docs.microsoft.com/pt-br/ef/)). De forma abstrata: o sistema passa a trabalhar da seguinte forma:

<center>
![ORM](https://www.devmedia.com.br/imagens/articles/233575/orm.png)
</center>

É possível encontrar um tutorial completo no [DevMedia](https://www.devmedia.com.br/entity-framework-tutorial/27764) sobre o Entity Framework, este que inclui os passos inicias e as abordagems de database, model e code first.

### Entendendo

Vamos utilizar a imagem anterior como base, e retratar duas áreas diferentes: a relacional e a orientada a objetos.

Na relacional, prevalece os princípios matemáticos, de forma que possamos armazenar e gerenciar os dados, de maneira segura. É nesta que utilizamos o SQL para dizer ao banco de dados `o que` fazer e **não** como fazer.

Na orientação a objetos, trabalhamos com calsses e métodos, logo, fundamentos da engenharia de software e que trabalham de maneira inversa ao relacional, dizem `como` fazer.

O ORM é justamente a ponte/ligação entre as duas áreas, desta forma, ele permite que você salve seus objetos (Orientação a Objetos) no banco de dados (Relacional).

Quase nunca é necessário escrever código SQL no ORM, já que o mesmo tem a responsabilidade de fazer isso pra você, em outras palavras: ele mapeia sua classe para o banco de dados, desta forma:

<center>
![Como o ORM trabalha](https://www.devmedia.com.br/imagens/articles/233575/ORM-Overview.png)
</center>
