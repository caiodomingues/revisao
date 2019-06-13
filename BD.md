# Revisão - BD

1. [SubQuery](#SubQuery)
2. [View](#View)
3. [PL/SQL](#PL/SQL)

## Considerações

>_**AVISO:** Eu não sei todo o conteúdo que vai cair na prova, escrevi somente o que me disseram que iria cair e o que deu tempo de escrever (¯\\\_(ツ )\_/¯). Rolaram uns boatos de Joins etc, mas, não tenho tempo de escrever tanta coisa assim, é noix :thumbsup:_


Considere a seguinte tabela para alguns exemplos do conteúdo:

```sql
CREATE TABLE funcionarios (...)
CREATE TABLE empresas (...)
CREATE TABLE cidades (...)
```

Com os seguintes valores prévios:

### Funcionários

| id | nome | salario | id_empresa | id_cidade |
|----|------|---------|------------|-----------|
|1|Thiago|1234,00|1|1|
|2|Maria|1234,00|2|2|
|3|João|1234,00|3|3|
|4|Pedro|1234,00|4|4|

### Empresas

| id | nome | id_cidade |
|----|------|-----------|
|1|Google|1|
|2|Apple|2|
|3|Facebook|3|
|4|GitHub|4|

### Cidades

| id | nome | população |
|----|------|-----------|
|1|Volta Redonda| 1234 |
|2|Barra Mansa| 4321 |
|3|Pinheiral| 1243|
|4|Resende| 3412 |

## SubQuery

O conceito de SubQuery é sua literal tradução lógica: utilizar uma query inferior, interna etc. Logo, uma Query é realizada _"dentro"_ 
de outra Query.

SubQueries são relativamente simples, basta imaginar que você deseja utilizar um valor que pode ser adquirido pela **Query X** na 
**Query Y**, por exemplo:

### Query X

Vamos supor que eu precise selecionar * de `funcionarios` de uma cidade específica, mas eu só tenho acesso aos nomes das cidades e a 
tabela referencia os IDs.

```sql
SELECT * FROM funcionarios WHERE id_cidade = ?
```

### Query Y

Sabemos então que dentro da tabela de `cidades`, podemos buscar pelo nome da cidade, assim, retornaremos também o ID, desta forma, a
seguinte query nos retorna o ID da cidade desejada.

```sql
SELECT id FROM cidades WHERE nome = 'Volta Redonda';
```

### Resultante

A Query final pode então ser tratada como a junção da **Query X** com **Y**, substituiremos o *?* em X por Y;

```sql

-- X
SELECT * FROM funcionarios WHERE id_cidade = ?

-- Y
SELECT id FROM cidades WHERE nome = 'Volta Redonda';

-- X com SubQuery
SELECT * FROM funcionarios WHERE id_cidade = (SELECT id FROM cidades WHERE nome = 'Volta Redonda');
```

Podemos realizar X SubQueries dentro de um SQL, como:

```sql
SELECT * FROM x WHERE id = (SELECT id FROM y WHERE id = 1) AND exemplo = (SELECT id FROM z WHERE id = 2);
```
_**OBS:** Utilizei nomes e variáveis aleatórias para agilizar o exemplo._

## View

A view pode ser definida como uma tabela virtual composta por linhas e colunas de dados vindos de tabelas relacionadas em uma query
(um agrupamento de SELECT’s, por exemplo). As linhas e colunas da view são geradas dinamicamente no momento em que é feita uma
referência a ela. 

Como já dito, a query que determina uma view pode vir de uma ou mais tabelas, ou até mesmo de outras views.

Ao criarmos uma view, podemos filtrar o conteúdo de uma tabela a ser exibida, já que a função da view é exatamente essa: 
filtrar tabelas, servindo para agrupá-las, protegendo certas colunas e simplificando o código de programação.

- **Reuso:** as views são objetos de caráter permanente. Pensando pelo lado produtivo isso é excelente, já que elas podem ser lidas por vários usuários simultaneamente.
- **Segurança:** as views permitem que ocultemos determinadas colunas de uma tabela. Para isso, basta criarmos uma view com as colunas que acharmos necessário que sejam exibidas e as disponibilizarmos para o usuário.
- **Simplificação do código:** as views nos permitem criar um código de programação muito mais limpo, na medida em que podem conter um SELECT complexo. Assim, criar views para os programadores a fim de poupá-los do trabalho de criar SELECT’s é uma forma de aumentar a produtividade da equipe de desenvolvimento.

```sql
CREATE OR REPLACE VIEW vwFuncionario AS
SELECT id AS ID_Funcionario,
       nome AS Nome_Funcionario,
       salario AS Salario_Funcionario,
       id_empresa AS ID_Empresa,
       id_cidade AS ID_Cidade,
FROM funcionarios;
```

Note a utilização do modificador `REPLACE`, este que força uma sobreposição do SQL caso a `vwFuncionario` já exista.

A consulta a estes dados é como uma consulta comum, dada por: `SELECT * FROM vwFuncionario;`. Podemos também inserir cláusulas do tipo
`WHERE` em nosso código de criação da View, bem como JOINs etc. Exemplo de View com Join e Where:

_Organizei melhor a estrutura do código para facilitar a visualização_

```sql
CREATE OR REPLACE VIEW vwFuncionario AS
SELECT f.nome as funcionario, e.nome as empresa, c.nome as cidade
FROM funcionarios f JOIN empresa e ON f.id_empresa = e.id JOIN cidade c ON f.id_cidade = c.id
WHERE f.id_cidade = 1;
```

Em uma transcrição do script, recriamos a view `vwFuncionario` selecionando o nome do funcionário, da empresa e da cidade, respectivamente.
Depois, dizemos de que tabela estamos buscando as informações, começando por funcionários e fazendo JOINs entre os campos comuns de 
id_emrpesa e id_cidade em `funcionario`. Por fim, retornamos os funcionários que trabalham na cidade de id igual a 1.

#### Resultado

| funcionario | empresa | cidade |
|----|------|-----------|
|Thiago|Google|Volta Redonda|

## PL/SQL

### Procedure e Function

Uma function é um bloco PL/SQL muito semelhante a uma procedure. O que podemos entender de início entre esses dois tipos de blocos é que os blocos functions retornam valores e as procedures podem ou não retornar um valor. As functions tem duas características que diferem das procedures, as quais não podemos deixar de tratar:

- As functions sempre retornam valores
- Functions são usadas como parte de uma expressão.

```sql
CREATE [OR REPLACE] FUNCTION function_name
  [(parameter_name [IN | OUT | IN OUT] type [, ...])]
  RETURN return_datatype
  {IS | AS}
  BEGIN
     < function_body >
  END [function_name];
```

- CREATE [OR REPLACE] FUNCTION: Caso uma function já exista com o mesmo nome, ela será reescrita devido ao termo ‘replace’. Caso contrário, ela será criada de acordo com o termo ‘create’.
- Function_name: Será o nome atribuído para essa função.
- Parameters: a lista opcional de parâmetros contém os nomes, os modos e os tipos que esses parâmetros terão. O IN representa o valor que será passado de fora, já o OUT representa que este parâmetro será utilizado para retornar um valor de fora do procedimento.
- Return_datatype: é o tipo de retorno que será utilizado, sendo este SQL ou PL/SQL. Podemos neste caso utilizar referências como o %TYPE ou %ROWTYPE se necessário, ou mesmo utilizar qualquer tipo de dados escalar ou composto.
- IS/AS: por convecção, temos o ‘is’ para a criação de funções armazenadas e o ‘as’ quando criamos pacotes (packages).
- function_body: contém o bloco PL/SQL que inicia com a claúsula BEGIN e finaliza com END [function_name], e executa neste momento todas as instruções necessárias.

```sql
CREATE OR REPLACE FUNCTION primeiro_nome_func
  RETURN VARCHAR(20);
 IS 
  emp_name VARCHAR(20); 
BEGIN 
SELECT primeiro_nome INTO emp_name
 FROM funcionarios_tbl WHERE ID = 90;
 RETURN emp_name;
END;
/
```

Há 3 formas de executar o código:

- Variável: `func_nome :=  primeiro_nome_func;`
- Select: `SELECT primeiro_nome_func FROM dual;`
- PL/SQL: `dbms_output.put_line(primeiro_nome_func);`

### Variáveis

Resumidamente: utilizadas para valores pré-determinados ou serem salvos e reutilizados em outras partes do código.

```sql
DECLARE
  var_dividend NUMBER := 24;
  var_divisor NUMBER := 0;
  var_result NUMBER;
BEGIN
...
END
```

Exemplo de uso em Procedure:

```sql
CREATE OR REPLACE PROCEDURE emp_sal_query (
    p_empno         IN CHAR(6)
)
IS
    v_lastname      VARCHAR2(15);
    v_job           VARCHAR2(8);
    v_hiredate      DATE;
    v_salary        NUMBER(9,2);
    v_workdept      CHAR(3);
    v_avgsal        NUMBER(9,2);
BEGIN
...
END
```

#### %TYPE

Nas variáveis, declaramos o tipo dela, como no exemplo anterior com o uso do modificador `NUMBER`, mas, caso queira fazer com que uma 
variável seja uma espécie "referência" ao mesmo tipo de dado::

```sql
CREATE OR REPLACE PROCEDURE emp_sal_query (
    p_empno         IN emp.empno%TYPE
)
IS
    v_lastname      emp.lastname%TYPE;
    v_job           emp.job%TYPE;
    v_hiredate      emp.hiredate%TYPE;
    v_salary        emp.salary%TYPE;
    v_workdept      emp.workdept%TYPE;
    v_avgsal        v_salary%TYPE;
BEGIN
...
END
```

### Exceptions

Exceptions são resultantes de erros, elas podem ser tratadas por meio do modificar `EXCEPTION`

```sql
DECLARE
   pe_ratio NUMBER(3,1);
BEGIN
   ...
EXCEPTION  -- Bloco de Exceptions
   WHEN ZERO_DIVIDE THEN  -- Trata erros de 'divisão por zero'
   ...
   WHEN OTHERS THEN  -- Trata todos os outros erros
   ...
END;  -- Exceptions e o Bloco terminam aqui
```
