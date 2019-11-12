# Revisão

Subprogramas = Procedures e Functions;

## Procedures

- Reaproveitamento de código – o código pode ser utilizado por diferentes aplicações
- Rapidez – O acesso é feito dentro do próprio banco de dados
- Controle de alterações – modificações realizadas em um só lugar
- Controle de acesso – pelas diretivas do banco de dados
- Modularização – possibilidade de organização em pacotes

### Diferença entre Blocos Anônimos e Subprogramas

? | Blocos Anônimos | Subprogramas
:--: | :--: | :--:
Nomeado | :x: | :heavy_check_mark:
Compilado | Todas as vezes | Apenas uma vez
Armazenamento de dados | :x: | :heavy_check_mark:
Invocação externa (outra aplicação) | :x: | :heavy_check_mark:
Retorno | :x: | :heavy_check_mark: (Funções)
Parâmetros | :x: | :heavy_check_mark:

### Características

- Nome único
- Nomenclatura: máximo 30 caracteres e iniciar por letra;

### Estrutura

- Cabeçalho
- Seção Declarativa
- Seção Executável
- Seção de tratamento de Exceções (opcional)

### Sintaxe

Note que a diferença léxica entre subprogramas e blocos anônimos é a existência de uma assinatura.

#### Bloco anônimo

**Nota: Exemplo sem todos os blocos (declare, exception)*

```sql
BEGIN
    -- código;
END [nome_da_procedure];
```

#### Subprograma

```sql
CREATE [OR REPLACE] PROCEDURE nome_da_procedure
    [(parametro1 [modo] tipo_de_dado1,
      parametro2 [modo] tipo_de_dado2, ...)]
IS | AS
    [declaracoes_de_variavel; ...]
BEGIN
    -- código;
END [nome_da_procedure];
```

> pausa pra propaganda AUHHAUHUA

![![Hackatomic](https://hackatomic.com)](../../ad.png)

## Parâmetros

Podem ser de três tipos:

- **IN**: Valores "entram" no subprograma por aqui;
- **OUT**: Retornam um valor para o chamador;
- **IN OUT**: Fornece um valor de entrada que pode ser retornado modificado;

Note que se o tipo de parâmetro não for informado, o **IN** será usado por padrão;

? | IN | OUT | IN OUT
:--: | :--: | :--: | :--:
Default | :heavy_check_mark: | :x: | :x:
Especificação de valor | No subprograma | Retornado para a chamada | Ambos anteriores
Parâmetros | O parâmetro formal funciona como uma constante | Variável **não** inicializada | Variável inicializada
Tipo de parâmetro (real) | Pode ser um literal, expressão, constante ou variável inicializada | **Deve** ser uma variável | **Deve** ser uma variável
Valor default | :heavy_check_mark: | :x: | :x:

### Formais X Reais

- Formais: é a declaração dos parâmetros na assinatura/especificação do subprograma:

    `CREATE PROCEDURE exemplo(p_id NUMBER)`

- Reais: são as expressões, variáveis ou literais usados na chamada do subprograma:

    `exemplo(1)`

### Especificando parâmetros na execução

É possível passar os parâmetros fora de ordem, mas você precisa especificar quem recebe o quê na chamada do subprograma:

#### Considere o seguinte subprograma

```sql
    CREATE OR REPLACE PROCEDURE exemplo (
        p_id IN employees.employeeID%TYPE,
        p_name IN employees.name%TYPE
    ) IS
    BEGIN
        ...
    END;
```

Na execução:

```sql
EXECUTE exemplo(p_name => 'Exemplo', p_id => 1);
```

### Valor padrão no parâmetro

É só atribuir direto na declaração do parâmetro:

```sql
CREATE OR REPLACE PROCEDURE exemplo (
        p_id IN employees.employeeID%TYPE DEFAULT 1,
        p_name IN employees.name%TYPE := 'Valor Padrão'
    ) IS
    BEGIN
        ...
    END;
```

![![Hackatomic](https://hackatomic.com)](../../ad.png)

## Functions

Assim como Procedures, é um bloco nomeado, mas que **retorna um valor**. Também pode ser armazenada no banco de dados para execuções repetidas e pode ser chamada como parte de uma expressão / fornecimento de valor para um parâmetro de outro subprograma.

A sintaxe é semelhante a de procedures, trocamos apenas a assinatura para `FUNCTION` e definimos um
`RETURN`:

```sql
CREATE [OR REPLACE] FUNCTION nome_da_funcao
    [(parametro1 [modo] tipo_de_dado1,
      parametro2 [modo] tipo_de_dado2, ...)]
RETURN tipo_de_dado
IS | AS
    [declaracoes_de_variavel; ...]
BEGIN
    -- código;
    RETURN expressao_de_retorno;
END [nome_da_funcao];
```

### Diferenças de Procedures e Funções

Antes comparamos blocos anônimos com subprogramas, agora, vamos comparar diretamente procedures e funções:

? | Procedures | Funções
:--: | :--: | :--:
Execução | Executados como uma instrução PL/SQL | Executados como parte de uma expressão
Cláusula de retorno | :x: | Obrigatório
Retorno de valor | Pode retornar caso haja parâmetros de saída | **Deve** retornar um **único** valor
Omissão de retorno | Podem conter um `RETURN` sem valor | **Devem** conter pelo menos uma instrução `RETURN`

### Execução

Como explicado anteriormente, funções são executados como parte de uma expressão, logo, devem ser executados da seguinte forma:

#### Procedure

`EXECUTE nome_da_procedure(p1, p2)`

#### Função

**Nota: usei o `DBMS_OUTPUT.PUT_LINE` como forma de exemplo! Não é mandatório;*

`EXECUTE dbms_output.put_line(exemplo(p1, p2))`

OU

**Nota: considerando que a função exemplo retorne um número;*

`VARIABLE b_salary NUMBER`
`EXECUTE :b_salary := exemplo(1)`

OU

`SELECT job_id, get_sal(employee_id) FROM employees`

![![Hackatomic](https://hackatomic.com)](../../ad.png)

## Packages

Um pacote é um objeto de esquema que agrupa logicamente tipos, variáveis e subprogramas PL/SQL relacionados.

É dividido em duas partes:

- Especificação
- Corpo

### Sintaxe

```sql
CREATE [OR REPLACE] PACKAGE nome_do_pacote
IS | AS
    public type e declarações de variáveis
    especificações de subprogramas
END [nome_do_pacote];
```

## Triggers

Mano, foram uns 8 trabalhos apresentados de triggers, não me faz escrever mais não.
