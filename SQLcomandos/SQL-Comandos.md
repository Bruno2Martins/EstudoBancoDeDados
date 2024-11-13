# SELECT

Visualizar tudo na tabela Clientes
```
SELECT * FROM Clientes
```

## order by
Visualizar tudo na tabela Clientes ordenado de a-z por nome (crescente)
```
SELECT * FROM Clientes
ORDER BY Nome 
```

Visualizar tudo na tabela Clientes ordenado de z-a por nome (decrescente)
```
SELECT * FROM Clientes
ORDER BY Nome DESC
```

Ordenar por ordem crescente o NOME e depois o SOBRENOME
```
SELECT * FROM Clientes
ORDER BY Nome, Sobrenome
```

## Colunas
Chamando algumas colunas, inves de tudo (tente chamar apenas o que ira usar)
SELECTE Nome, Sobrenome, Email FROM Clientes

## WHERE
Filtrar a busca

Mostrar clientes com nome Adam
E sobrenome Reynolds
```
SELECT * FROM Clientes
WHERE Nome = 'Adam' AND Sobrenome = 'Reynolds'
ORDER BY Nome, Sobrenome
```

Mostrar clientes com nome Adam
OU sobrenome Reynolds
```
SELECT * FROM Clientes
WHERE Nome = 'Adam' OR Sobrenome = 'Reynolds'
ORDER BY Nome, Sobrenome
```

Mostrar clientes que aceitam comunicado
```
SELECT * FROM Clientes
WHERE AceitaComunicados = 1
ORDER BY Nome, Sobrenome
```

## LIKE

Mostrar clientes que começa com G
```
SELECT * FROM Clientes
WHERE Nome LIKE 'G%'
ORDER BY Nome, Sobrenome
```

Se quiser mostrar clientes que tenham G independente da posição
```
SELECT * FROM Clientes
WHERE Nome LIKE '%G%'
ORDER BY Nome, Sobrenome
```

# INSERT

No c# é " mas no sql é '

Nomes das colunas da tabela e os dados, a ordem do nome da coluna que colocar irá corresponder com a ordem do dado.
```
INSERT INTO Clientes (Nome, Sobrenome, Email, AceitaComunicado, DataCadastro)
VALUE ('Bruno','Martins', 'email@email.com',1, GETDATE())
```
no INSERT você tem a opção de esconder os nomes da tabela na hora da inserção, porem deverá obedecer as colunas da tabela 
```
INSERT INTO Clientes VALUE ('Bruno','Martins', 'email@email.com',1, GETDATE())
```

# UPDATE

Atualizar o dado da tabela.

Defina no SET os campos e valores que quer atualizar, no WHERE seleciona qual é o registro que quer atualizar

```
UPDATE Clientes
SET Email = 'emailatualizado@email.com'
WHERE Id = 1001
```
**_!!! CUIDADO, TENHA CERTEZA DE QUE O QUE VOCÊ ESTA ATUALIZANDO, É EXATAMENTE O QUE QUER ATUALIZAR!!!_**

> Dica: faça um select daquilo que vai atualizar primeiro, para ter certeza que é o registro correto.

para voltar um update é com backup
o update é final!

## UPDATE SEM WHERE

#### BEGIN TRAN
* quando coloco isso, é um modo que posso desfazer as minhas alterações

#### ROLLBACK
* voltar para quando o BEGIN TRAN foi execultado


```
SELECT * FROM Clientes

BEGIN TRAN

UPDATE Clientes
SET Email = 'emailatualizado@email.com'

ROLLBACK
```

# DELETE
Deletar registro da minha tabela.

Para isso: DELETE, o nome da tabela e a condição
```
DELETE Clientes
WHERE Id = 1001
```
**_!!! CUIDADO, TENHA CERTEZA DE QUE O QUE VOCÊ QUER DELETAR!!!_**


# CRIAÇÃO DE TABELA
CREATE TABLE nome (colunas)

essas colunas deve ser de um tipo, temos:
## Tipos de dados
### Dados tipo **STRING**

Geralmente 
varchar(n) ou char(n), porém temos vários outros.
- ex. char: quando tem certezo do tamanho de caracter, como sigla de estado, tenho certeza que terá 2 caracteres.
- ex. varchar: quando não tenho certeza do tamanho de caracteres, 

### Dados tipo **NUMERIC**

- bit - tipo boleano 0, 1 ou NULL

- int
    - tynyint
    - smallint
    - bigint
- decimal(p,s) - valores monetarios, sendo o mais recomndavel para isso.
    - p é a quantidade de números 
    - s é a quantidade de casa decimal

- numeric(p,s)
- smallmoney
- money
- float(n)
- real

### Dados **DATE e TIME**

- datetime
- datetime2 
    - mais preciso, recomendavel
- smalldatetime
- date
- time
- datetimeoffset
- timestamp

## Crinado a tabela
```
CREATE TABLE Produtos(
    Id int IDENTITY(1,1) PRIMARY KEY NOT NULL,
    Nome varchar(255) NOT NULL,
    Cor varchar(50) NULL,
    Preco decimal(13, 2) NOT NULL,
    Tamanho varchar(5),
    Genero char(1)
)
```
IDENTITY -  o campo vai ser auto incrementado, o banco de dados vai cuidar disso 

PRIMARY KEY - terá um dado único, para servir como identificador do registro

NOT NULL - significa que o campo é obrigatorio, deve ser preenchido

NULL - campo não obrigatorio ,pode ser explicito