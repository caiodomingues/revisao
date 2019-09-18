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

Não encontrei nada sobre diagrama de classe de projeto nas apostilas (2 fala somente sobre a de Domínio e 3 só cita a de Domínio).

### Edit

> Me disseram que tá na apostila 3 (pág 16 aproximadamente), mas eu vou manter o que eu já tinha escrito aqui porque deu trabalho :thumbsup:, se tá certo ou não, eu não faço ideia, é noix.

---

> Todo o conteúdo a partir daqui foi retirado da internet. Inseri todas as fontes de pesquisa nos seus respectivos pontos, não assumo qualquer responsabilidade sobre seus resultados em provas visto que a minha interpretação sobre as informações podem estar incorretas.

Me baseando em outras pesquisas pela internet, entendo que outras formas de nomear o Diagrama de Classe de Domínio é chamá-lo também de **Modelo de Domínio** ou ainda **Modelo Conceitual**.

O Diagrama de Classes de Projeto define classes como componentes de software / classes de software. **Fonte:** [**USP**](https://edisciplinas.usp.br/pluginfile.php/383727/mod_resource/content/2/Aula07_DiagramaDeClasse.pdf)

Desta forma, como o Modelo de Domínio não representa os componentes de um Software, é de responsabilidade do Diagrama de Classe de Projeto fazer.

O diagrama de classes evolui com o sistema
e pode ter diferentes perspectivas:

- Na análise – identificamos objetos (classes)
no domínio do problema
- No projeto – pensamos em objetos (classes)
para a solução

Isto é:

- O modelo conceitual (análise) representa as
classes no domínio do negócio em questão. Não
leva em consideração restrições inerentes à
tecnologia a ser utilizada na solução de um
problema.
- O modelo de classes de especificação (projeto) é
obtido através da adição de detalhes ao modelo
anterior conforme a solução de software escolhida.

**Fonte:** [**UFPR**](http://www.inf.ufpr.br/silvia/ES/projeto/aulas/aula5.pdf)

Recomendo leitura de outros conteúdos como este do [Daniel Cavalcanti](https://danielcavalcanti.com.br/home/diagrama-de-classes/).
