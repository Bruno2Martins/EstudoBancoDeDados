# BUILT-IN FUNCTIONS
São funções pré-existentes que auxiliam na manipulação de dados, como por exemplo contar, somar, media, etc...

## COUNT

Conta quantas linhas tem na tabela. 
Aqui os dados não importam, apenas a quantidade de linhas.
```
SELECT COUNT(*) FROM Produtos
```

Por padrao quando se executa uma function ela n tem nome, mas pode dar um nome para ela.
```
SELECT COUNT(*) QuantidadeProdutos FROM Produtos
```

Pode dar uma condição para o que contar.
```
SELECT COUNT(*) QuantidadeProdutosTamanhoM FROM Produtos WHERE Tamanho = 'M'
```

## SUM

Função Soma, funciona apenas em colunas que são do tipo inteiro e demais
```
SELECT SUM(Preco) PrecoTotal FROM Produtos
```
Pode dar uma condição para o que somar.
```
SELECT SUM(Preco) PrecoTotalProdutosTamanhoM FROM Produtos WHERE Tamanho = 'M'
```

## MAX, MIN, AVG
Para saber o valor *maximo* de determinada tabela
```
SELECT MAX(Preco) ProdutoMaisCaro FROM Produtos
SELECT MAX(Preco) ProdutoMaisCaro FROM Produtos WHERE Tamanho = 'M'
```
Para saber o valor *minimo* de determinada tabela
```
SELECT MIN(Preco) ProdutoMaisBarato FROM Produtos
SELECT MAX(Preco) ProdutoMaisBarato FROM Produtos WHERE Tamanho = 'M'
```
Para saber o valor *medio* da tabela
```
SELECT AVG(Preco) MediaProdutos FROM Produtos
```

## Concatenando Colunas
Se quiser mostrar dados de duas colunas em apenas uma, podemos concatenar.
Do mesmo modo podemos colocar um nome para essa junção
```
SELECT 
    Nome + ', Cor:' + Cor NomeProduto
FROM Produtos
```
## UPPER e LOWER
Para deixar as letras todas maiusculas
```
SELECT UPPER(Nome) Nome FROM Produtos
```
Para deixar as letras todas minusculas
```
SELECT LOWER(Cor) cor FROM Produtos
```

## Adicionando uma nova coluna
Pode se fazer por script e de maneira visual

```
ALTER TABLE Produtos ADD DataCadastro DATETIME2
```
Aqui alteramos a tabela e adicionamos a coluna DataCadastro com o tipo DATETIME2
```
UPDATE Produtos SET DataCadastro = GETDATE()
``` 
Fiz um UPDATE obtendo a data atual de toda a coluna
GETDATE() - uma função do sql que pega a data e hora atual da sua maquina

Para remover a coluna:
```
ALTER TABLE Produtos DROP COLUMN DataCadastro
```

## Formatando uma data

Utiliza a função FORMAT no nome da coluna e como você quer representar a data

```
SELECT 
    Nome + ', Cor:' + Cor NomeProduto
    FORMAT (DataCadastro, 'dd/MM/yyyy HH:mm') Data
FROM Produtos
```

## GROUP BY
Agrupamento de dados
Agrupar dados q são iguais e depois contar quantos agrupamentos eu tenho feito.

Ex: agrupar todos os produtos de tamanho m e contar depois agrupar todos os produtos de tamanho g e contar

```
SELECT 
    Tamanho,
    COUNT(*) Quantidade
FROM Produtos
GROUP BY Tamanho
```

temos uma condição para tirar o que nao mostar os tamanhos vazios
> <> sinal de difererente, assim como !=
```
SELECT 
    Tamanho,
    COUNT(*) Quantidade
FROM Produtos
WHERE Tamanho <> ''
GROUP BY Tamanho
ORDER BY Quantidade DESC
```
Finalizado ordenando do maior para o menor.
Aqui a ordem dos comandos é importante, para seguir uma lógica

## PRIMARY KEY e FOREIGN KEY

PRIMARY KEY: Chave única que indentifica cada registro na tabela.
Uma chave primaria

FOREIGN KEY: Chave que identifica um registro existente em outra tabela.
Uma chave estrangeira

### Criando a tabela de endereços com FOREIGN KEY

```
CREATE TABLE Enderecos (
    Id int PRIMARY KEY IDENTITY(1,1) NOT NULL,
    IdCliente int NULL,
    Rua varchar(255) NULL,
    Bairro varchar(255) NULL,
    Cidade varchar(255) NULL,
    Estado clar(2)NULL

    CONSTRAINT FK_Enderecos_Clientes FOREIGN KEY(IdCliente)
    REFERENCES Clientes(Id)

)
```

Inserindo dados para testar
```
INSERT INTO Enderecos VALUES (4,'Rua Teste', 'Bairro Teste', 'Cidade Teste', 'SP')

SELECT * FROM Clientes WHERE Id = 4
SELECT * FROM Enderecos WHERE IdCliente = 4

```

## JOIN
*INNER JOIN* é uma junção de tabelas. 
Para que eu traga a informação em apenas um SELECT. 
Deve ter uma condição para ter essa junção, a chave estrangeira de uma tabela com a chave primaria de outra tabela que estejam relacionadas.


```
SELECT * FROM Clientes WHERE Id = 4
SELECT * FROM Enderecos WHERE IdCliente = 4
```
seria
```
SELECT 
    * 
FROM 
    Clientes
INNER JOIN Enderecos ON Clientes.Id = Enderecos.IdCliente
WHERE Clientes.Id = 4 
```

No inner JOIN, se voce quer selecionar determinado dado, pode ser que seja ambiguo se tiver nas duas tabelas, então identificar de qual tabela, se n houver coluna ambigua, é um select normal

se houver ambiguidade:
>SELECT Clientes.Id...

se não:
>SELECT Nome ...

Pode chamar de outra maneira, dando uma abreviação para o nome da sua tabela. Ex.: Clientes C.

```
SELECT 
    C.Nome,
    C.Sobrenome
    E.Rua,
    E.Bairro
FROM 
    Clientes C
INNER JOIN Enderecos E ON Clientes.Id = Enderecos.IdCliente
WHERE Clientes.Id = 4 
```
### Outros Joins
Temos tambem 
- LEFT JOIN
- RIGHT JOIN
- FULL OUTER JOIN