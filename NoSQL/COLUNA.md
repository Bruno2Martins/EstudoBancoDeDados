<a href = "README.md">Voltar</a>
<hr>

# COLUNA

Armazenam as informaçoes exatemante nas suas colunas de forma independente entre elas.

Hierarquia

diferenca de terminologias

- **Keyspace**: agrupamento de familias de colunas -> database
- **Column Family/table**: agrupamento de colunas -> table
- **Row key**: chave que representa uma linha de coluna -> Primary Key
- **Column**: representa um valor contendo: Name, Value Timestamp
#
O uso ideal é realizar buscas pela chave na consulta, como:

**Registro de transações**: compras, resultados de testes, filmes assistidos e localização mais recente do filme.

Rastreando praticamente qualquer coisa, incluindo status do pedido/pacotes etc.

# Prática:
Utilizando o Cassandra iremos criar uma estrutura e realizar operações.

cql - linguagem do cassandra, cassandra query language 

<a href ="https://katacoda.com/datastax/courses/cassandra-try-it-out/try-cql/">SITE PARA PRATICA</a>

*Para mais aprofundamento, fazer a instalação do Cassandra no ambiente local*

### INICIANDO A CRIAÇÃO DO KEYSPACE
```
CREATE KEYSPACE fenda_biquini WITH replication = {'class': 'SimpleStrategy', 'replication_factor':1};
// criei a keyspace que seria a base de dados, com propriesdade de replicação, passando um json para essas estrategias, com classe e o fator 

use fenda_biquini;
//definir qual keyspace usar

CREATE COLUMNFAMILY clients (name TEXT PRIMARY KEY, age int);
SELECT * FROM clients;
//criei a column family que seria a tabela, com o nome sendo a chave primaria, e uma idade

INSERT INTO clients (name, age) VALUES ('Bob Esponja',38);
SELECT * FROM clients;
// inserindo os valores do registro para o clients

INSERT INTO clients JSON '{"name": "Patrick"}';
SELECT * FROM clients;
// inserindo de forma JSON o registro para clients

SELECT age, WRITETIME(age) FROM clients;
//podemos ver que por mais que traga 2 linhas pois temos 2 registros em clients, no nosso 2 registro não tras nem idade nem timestemp, significando que a coluna "age" possui apenas um registro

SELECT * FROM clients HERE name = 'Bob Esponja';

SELECT JSON * from clients;

UPDATE clients SET age=33 WHERE name = 'Patrick';
// add idade para o patrick

ALTER COLUMNFAMILY clients ADD hobby text;
//alterar 'tabela' adicionando coluna hobby

UPDATE clients SET hobby='Caçar agua viva' WHERE name ='Patrick';
SELECT * FROM clients;

SELECT age, WRITETIME(age), hobby, WRITETIME(hobby) FROM clients WHERE name='Patrick';
//tras as colunas com seus timestemp: vemos que não sai atualizando a linha inteira, apenas aquela coluna, pois as colunas são individualizadas entre si

DELETE FROM clients WHERE name='Bob Esponja';
SELECT * FROM clients;
// da mesma forma que no relacional, podemos excluir registros

