# Revisão - ASP.NET CORE

1. [Introdução](#Introdução)
    1. [Model](#Model)
    2. [View](#View)
    3. [Controller](#Controller)
2. [](#)
3. [](#)
4. [](#)

## Introdução

Para começar precisamos entender o que o padrão `MVC` significa.

- M = Model 
- V = View
- C = Controller

Este modelo foi criado para aplicar um dos principios mais famosos entre os programadores que é o `DRY` (Don't Repeat Yourself - 
Não se repita).

## Model

Os modelos são as classes da sua aplicação, definidas como visto pela matéria  de Análise de Projeto Orientado a Objetos.

> *pausa para os risos*

![class-diagram](https://aec67204-a-62cb3a1a-s-sites.googlegroups.com/site/mindbitufrpe/diagramas/diagrama-de-classes/Class%20Diagram0.png)

Nestes modelos você precisa definir todos os atributos que você vai precisar em cada classe.

## View

View é o layout da aplicação, ou seja, o que o cliente vai "ver" na tela.

É possivel trazer uma variavel e/ou atributo criada do modelo para a view, isto significa que sempre que você trocar no seu modelo isto vai automaticamente trocar na sua view também, ou seja, uma aplicação dinâmica. O `Razor` é a View Engine responsável pelo tratamento das informações para a página, ele permite a inserção de código C# em conjunto com o HTML de uma view.

-------------- Seria bacanas umas imagens de exemplos de view aqui.

Dentro da pasta View da sua aplicação haverá uma pasta `Shared`. A partir desta, podemos entender melhor o porque a introdução do conceito `DRY`.

A pasta `Shared` é responsável por manter todos os arquivos de layouts comuns de páginas, uma espécie de componentização de suas views. Exemplo:

Se um site possui uma navbar (menu) em todas as outras páginas, você não precisa reescrever o mesmo código várias vezes, bem como não precisa fazer alguma alteração em diversos arquivos simultâneamente, um único arquivo de layout pode conter este trecho de código da navbar, que é posteriormente importada pelas outras views, facilitando a manutenção e usabilidade do código. Por padrão, há um arquivo chamado `_Layout.cshtml` na pasta `Shared`. 

## Controller

Controllers são responsáveis por realizar as funções da página, ou seja, este controllers são os responsáveis por saber qual view pertence a qual modelo e para onde vai os dados recebidos pelo cliente ou pelo servidor. Isto é, ser responsável por tratar as requisições, sejam estas GET, POST etc.

------- Foto do padrão de MVC aqui.
