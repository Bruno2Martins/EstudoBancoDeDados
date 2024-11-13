<a href = "README.md">Voltar</a>
<hr>

# CHAVE-VALOR

constituido em duas partes
Armazena um conjunto de dados, seja ele simples ou complexo,
idendificados por um identificador exclusivo.

+Bom desempenho em aplicações na nuvem.

-menor capacidade de busca.

Uso: cache, sessao de usuario, carrinhos de compras.

# Prática
Iremos utilizar o Redis, um banco de dados, cache, messageria e fila.
- Alto desempenho
- Estrututar de dados na memoria.
- Versatilidade de uso.
- Replicação e persistência.

<a href="https://try.redis.io">SITE TRY REDIS</a>

```
SET user1:name "Bob Esponja"
//add nome do user 1
GET user1:name
//retorna: Bob Esponja
```
```
SET user2:name "Lula Molusco" EX 10
//EX - tempo de expiração dos dados registrados, em segundos
GET user1:name
// durante 10 seg mostra o dado, apos isso, não mais
```
```
//EXISTS - verifica se há registros na chave 

EXISTS user2:name
//retorna: (integer) 0
EXISTS user1:name
//retorna: (integer) 1

```
```
//
LPUSH user1:hobbies "Caçar agua viva"
//retorna: (integer) 1
LPUSH user1:hobbies "Comer hamburgues"
//retorna: (integer) 2
GET user1:hobbies
//da erro pois temos dois registros em hobbies, definir qual deseja exibir.

//para acessar os dados temos 2 formas, pelo indice e por base de range

//pelo indice:
LINDEX user1:hobbies 1
// retorna o hobbie 1 "Caçar agua viva"
LINDEX user1:hobbies 2 
// retorna o hobbie 2 "Comer hamburgues"

//pelo range
LRANGE user1:hobbies 0 1
/*retorna:
1) "Comer hamburgues"
2) "Caçar agua viva"
*/
```
```
// COMANDO TYPE
// para descobrir o tipo do registro na propriedade
TYPE user1:hobbies
//retorna: "list"
```
```
TTL - tempo de expiração do nosso objeto em segundos
PTTL - tempo de expiração do nosso objeto em milisegundos

>TTL user2:name
//retorna: (integer) tempo atual para expiração em segundos
```
```
PERSIST - remove o tempo de expiração

>PERSIST user2:name
```
```
DEL - deletar a chave

DEL user2:name
```
