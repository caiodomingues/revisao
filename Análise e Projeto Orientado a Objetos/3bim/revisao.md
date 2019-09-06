# Revisão

Diagrama de Classe de:
1. [Domínio](#Domínio)
2. [Projeto](#Projeto)

## Domínio

Apostila 2 (Primeira aula do portal), página 31.

O diagrama de classes demonstra a estrutura estática das classes de um sistema. Esta que por sua vez representa o que é gerenciado pelo sistema, desta forma precisamos de:

- Identificar as classes;
- Identificar os atributos das classes, sem levar em conta sua visibilidade ou tipos;
- Analisar os atributos, identificando que alguns deles são na realidade relacionamentos;
- Algumas operações podem ser identificadas neste ponto, mas com certeza, não todas. As operações são descobertas com auxilio de outros diagrama;
- Analisar classes semelhantes, remodelando-as com relacionamentos de herança;
- Lançar detalhes dos atributos.

Classes podem se relacionar com outras através de diversas maneiras:

- Associação (conectadas entre si);
- Dependência (uma classe depende ou usa outra classe);
- Especialização (uma classe é uma especialização de uma outra classe);
- Pacotes (classes agrupadas por características similares).

Eu achei um [vídeo do Prof. Rosenclever](https://www.youtube.com/watch?v=eiMS2xGno88) sobre estes diagramas. Se preferir ler, eu transcrevi o vídeo:

<center>

[![YouTube](https://img.youtube.com/vi/eiMS2xGno88/0.jpg)](https://www.youtube.com/watch?v=eiMS2xGno88)

<small>Clique na imagem para abrir o vídeo no YouTube.</small>
</center>

O diagrama de classe de domínio é usado para mapear os objetos que vão existir para resolver o problema / sistema.

> Use a imagem acima como base para a transcrição da aula.

Primeiro se cria a classe de `Pedido`.
A classe `Cliente` representa os dados do `cliente` que vão estar no `Pedido`.
O `Cliente` compra produtos, então, cria-se a classe produtos.

A seta está para o lado do `Cliente` pois o `Pedido` tem a informação do `Cliente`, pois quando você vai registrar um `cliente`, você não tem informação do pedido dele.

Na etapa de análise, podemos fazer uma associativa, que é relacionar `Pedido` com Produto, e essa classe associativa se torna `ItemPedido`, que é o 
Produto daquele `Pedido`.

Um `Pedido` é realizado por um `Cliente`, então, atribuímos 1 no lado do `Cliente`, e este pode fazer `0-N` pedidos, logo:

    Pedido (0, n) -> (1) Cliente

E para a relação de pedido e produtos:

    Pedido (0, n) <- (1, *) Produto

Por fim, vamos definir os atributos **SEM DEFINIR O TIPO DE DADO** destes, desta forma:

| Pedido | Produto | ItemPedido | Cliente |
| - | - | - | - |
| data | estoque | quantidade | nome |
|  |  | preco |  |

> Eu não sei porque a imagem de exemplo na apostila do Venício tá diferente do que a própria explicação em texto, mas ok, qualquer pesquisa no google retorna o uso de atributo, bem como o próprio texto dele define o uso de atributos sem definição de tipo ou visibilidade (Segundo tópico na definição das classes de domínio) 

## Projeto

