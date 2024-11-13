<a href = "README.md">Voltar</a>
<hr>

# DOCUMENTO
Dados e documentos autocontidos e auto descritivos.

Permite redundância e inconsistência.

Livre de esquemas podendo utilizar JSON, XML entre outros.

# O MongoDB

- Codigo aberto
- Alta performace
- Schema-Free
- Utiliza json para armazenar dados
- Suporte a indices
- Auto-Sharding 
- Map-Reduce
- GridFS

## Estruturação
Document ==> Tupla/registro

Collection ==> Tabela

Embedding/linking ==> Join

## Quando Usar:
- Grande volume de dados.
- Dados não necessariamente estruturados.

## Quando não Usar
- Necessidade de relacionamentos/joins
- Propriedades ACID e transações não importantes.

***Curiosidade***: Diversas entidades de pagamento não homologam sistemas cujos dados financeiros dos clientes não estejam em banco de dados relacionais tradicionais.

# Começando a instalação
```
vi docker-composer.yml
version: '3.8'

services:
    db:
        image: mongo
        container_name: db
        restart: always
        environments:
            - MONGO_INITDB_ROOT_USERNAME=dio
            - MONGO_INITDB_ROOT_PASSWORD=dio
        ports:
            - "27017:27017"
        volumes:
            - /Users/bruno/DIO/dbdata:/data/db

            //salvar
docker-compose up -d
docker container ps

mongo --host (meuip):27017 -p dio -u dio

show databases
exit

```
Client que podemos conectar com o mongodb para melhor visualização: 
<a href="https://www.robomongo.org">Robo 3T</a>

# MongoDB Cloud
mongodb.com

é uma ferramenta paga, porem há o teste gratuito, com funcionalidades limitadas

### apos cadastro
>Build Cluster > free > Create Cluster

>Network Access > Add Ip Address > Add Current Ip Address > Confirm 

>Database Acess > Add New Database User > (password > user e pw > read and write to any database) > Add User

Assim com o ip ja liberado, podemos acessar de forma remota o nosso cluster do mongo

>Clusters > Connect > Connect with the mongo shell > (copie o comando)
comando:
```
mongo "mongodb+srv://cluster0.vleov.mongodb.net/myFirstDatabase" --username <username>
```

no terminal colar o comando assim se conectando

### Outro client do mongo
MongoDB Compass

# Schema Design
É free, porem temos boas praticas.

Embedding vs Referência

embedding
documentos autocontido
pros:
    consulta informações em uma unica query
    atualiza o registro em uma unica operação
    atomicidade

contras:
    limite de 16MB por documento

Referência
documentos com dependencia de outros documentos ou collections
pros:
    documentos pequenos
    não duplica informações
    *Usado quando os dados não são acessados em todas as consultas*
contras:
    duas ou mais queries ou utilização do $lookup

Recomendações de uso dos relacionamentos:
one-to-one: prefira atributos chave-valor no documento
one-to-few: prefira embedding
one-to-many e many-to-many: prefira referência


## BOAS PRATICAS
- Evite documentos muito grandes
- Use nome campos objetivos e curtos
- Analise as suas queries utilizando explain()
- Atualize apenas os campos alterados
- Evite negações em queries

Listas/Arrays dentro dos documentos não podem crescer sem limite

# JSON vs BSON

BSON
é uma serialização codificada em binario de documentos semelhantes a JSON.
Contem extensçoes que permitem a representação de tipos de dados que não fazem parte da especificação JSON. Por exemplo, BSON tem um tipo Date, ObjectID

# Operações de manipulação de dados

se cria uma collection de forma explicita, vc pode colocar algumas validaçoes nela, de forma implicita não

```
mongo --host (ip):27017 -u dio -p dio

show databases;

// para criar um db novo basta usar o comando 'USE' e o nome do db que você deseja, se o db já existir o mungo muda para ele, se não ele cria e muda.
use fenda_biquini; 
db.createCollection("test", {capped: true, max: 2, size: 2});
show collections; // para verificar

db.test.insertOne({"name":"Teste 1"});
db.test.find({}); // para listar todos os documento da collection
db.test.insertOne({"name":"Teste 2"});

db.test.insertOne({"name":"Teste 3"});
// pode apenas 2 collections, então após maximo, ele exclui o registro mais antigo e coloca um novo, no caso tira o teste 1 e agr temos o teste 2 e 3 

// criando de forma implicita
db.test1.insertOne({"age": 10});
db.test1.find({});

db.test1.insertOne({"age": 10});
db.test1.insertOne({"age": 10});
db.test1.insertOne({"age": 10});
db.test1.insertOne({"age": 10});
db.test1.find({});
// se executar quantos achar necessario, quando quiser chamar trara todos





db.clients.insert([{"name": "Patrick", "age": 38},{"name": "Bob Esponja"}])
//essa forma de inserção é chamado bulkWriter

db.clients.find({});
//aqui verificamos que foram criados os 2 documentos
//(aqui mostra o id e a as informações criadas na collection, ex: "_id": ObjectId("idunico123"), "name": "Patrick", "age": 38) 

//comando SAVE - se passar um doc ou um id que ja existe ele realiza a atualização, ou se n existe ele realiza a inserção. Porem atualiza o documento por completo, que não é uma boa pratica.

db.clients.save({"_id": ObjectId("idunico123"), "name": "Patrick", "age": 40});
//mostrara que modificou o documento, aqui fiz o Patrick ter 40 anos.

db.clients.save({"name": "Lula Molusco", "age": 40});
// se der um save para um objeto que não existe, apresenta que fez a inserção de um documento.



//comando UPDATE - é composto por algumns parametros, a query de match, o criterio de seleção dos documento da atualização, e de segundo parametro, quais informações queremos atualizar, e ainda se sera uma alteração multipla, alem de outros parametros disponiveis

db.clients.find({});
db.clients.update({"name": "Bob Esponja"}, {$set:{"age": 41}});
// nesse caso o criterio foi o documento que tem o nome bob esponja, é para alterar e colocar uma idade para 41

db.clients.update({"age": 40}, {$set:{"age": 43}});
//aqui ele encontra o primeiro que tem age 40 e altera
db.clients.find({});

db.clients.update({"age": 40}, {$set:{"age": 43}}, {multi: true});
//aqui ele encontra todos que tem age 40 e altera
db.clients.find({});

db.clients.update({"age": 43}, {$set:{"age": 44}}, {multi: true});
//aqui ele encontra todos que tem age 43 e altera
db.clients.find({});




// comando UPDADE MANY - a partir da versão 3.2 podemos simplificar o update multi passando apenas 2 parametros, a query de match e o que vamos atualizar
db.clients.updateMany({"age": 43}, {$set:{"age": 44}});


//no comando find podemos escolher o que estamos procurando, colacar quantas informações queremos, etc... seria o select do db relacional
db.clients.find({"age": 44}).limit(1);
db.clients.find({"age": 44, "name": "Lula Molusco"});


// operadores logicos e de comparação:
//IN - para que os documentos façam match
//OR - ou
//LT - pegar dados menor do valor definido
//LTE - pegar dados menor ou igual do valor definido

db.clients.insertOne({"name": "Patrick2", "age": 30})
db.clients.find({});

db.clients.find({"age": {$in [30,41]}});
db.clients.find({$or [{"name": "Lula Molusco"},{"age":41}]});
db.clients.find({"age":{$lt:43}});
db.clients.find({"age":{$lte:44}});




//DELETE
//deleteOne - deleta o primeiro documento encontrado pelo parametro
//deleteMany - deleta o todos os documento encontrado pelo parametro

db.clients.find({});
db.clients.deleteOne({"age": 55});
//deleta o primeiro documento 
db.clients.deleteMany({"age": 55});
db.clients.find({});
```

<a href="https://docs.mongodb.com/manual/tutorial/query-for-null-fields">Documentação do MongoDB</a> para mais operações de manipulação de dados


# Performace e Índices

Índice - o índice dá um direcionamento no nosso banco para onde esta o dado, serve para evitar um collection scan, no relacional um table scan

O mongo ja cria sozinho um indice para os IDs dos documentos, mas podemos criar índices tambem.

Podemos criar índices compostos, únicos, por ordenação, tem como criar não só para melhorar consultas, mas para garantir integridades

O mongo permite que façamos comandos JS dentro dele

## Criando um índice:

criando um indice para o nome
```
db.getCollection('clients').createIndex({name:1}, {"name": "idx_name"})
```

# Agregações
## Conceito
Agregação é o procedimento de processar dados em uma ou mais etapas,onde o resultado de cada etapa é utilizafo na etapa seguinte, de modo a retornar um resultado combinado.

Agregações de proposito únicos
- count
- distinct

Elas não permitem as costumizações das agregações utilizando pipeline
```
db.getCollection('restaurants').count({})
db.getCollection('restaurants').distinct("cousine")
```

- Operadores: $group, $addFields entre outros

**$group**
```
db.getCollection('restaurants').aggregate([{$group: {_id? "$cuisine", total: {$sum: 1}}}])
```
**$addFields**
```
db.getCollection('restaurants').aggregate([{$addFields: {"teste": true}}])
//apenas adiciona o campo no resultado da sua agregação
db.getCollection('restaurants').find({})
//pode ver aqui que não existe o campo teste criado anteriormente

```

- Funções : $sum, $avg, $max, $min
- Operadores Lógicos: $and, $or, $not e $nor

para usar devemos utilizar um filtro.

Filtro na agregação:
```
db.getCollection('restaurants').aggregate([{$match : {$and:[{cuisine: "American"}, {borough: "Brookyn"}]}}])

db.getCollection('restaurants').aggregate([{$match : {$or:[{cuisine: "American"}, {borough: "Brookyn"}]}}])

```

- Operadores de comparação:
    - Maior que = $gt
    - Menor que = $lt
    - Diferente de = $nte
    - Igual = $eq
    - Maior ou igual = $lte
    - Menor ou igual = $gte
