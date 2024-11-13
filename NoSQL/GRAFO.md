<a href = "README.md">Voltar</a>
<hr>

# GRAFOS

Grafos são estruturas matematicas composta de nós e vertices.

Dentro do banco de dados, *nós* seriam os **dados** e as *vertices* os **relacionamentos**

Comum em detecção de fraudes, mecanismos de recomentdação, redes sociais, sistemas de arquivos, games...

## Pratica

Vamos criar uma estrutura de registros que compóem os dados de uma rede social utilizando um sandbox do Neo4j

O Neo4j utiliza a linguagem cypher como forma de linguagem e estruturação dos seus dados.

<a href ="https://sandbox.neo4j.com/">SANDBOX DO NEO4J</a>

Simulando uma rede social
```
CREATE(:Cliente {name: "Bob Esponja", age: 28, hobbies: ['Caça agua-viva, Comer hamburgues']})
```
> assim criou uma label, um nó, com um SET de 3 propriedades


### > Forma de consultar
```
MATCH (bob_esponja) RETURN bob_esponja
```

### > Criando um nó

```
CREATE (:Client {name "Lula Molusco", age: 30, hobbies: ['Tocar clarinete']}) -[:Bloqueado]->(:Client {name: "Patrick", hobbies: ['Caçar agua viva']})
```
> criou 2 label, 2 nos, setou 5 propriedades e criou um relacionamento

### > Criar um novo relacionamento
```
CREATE (:Object)

MATCH (lula:Client {name:"Lula Molusco"}), (patrick:Client {name:"Patrick"}) CREATE (lula)-[:Bloqueado] ->(patrick)

```

### > Excluir relacionamento

```
MATCH (lula:Client {name:"Lula Molusco"})-[relaciona:Bloqueado]-() DELETE relaciona
```

### > excluindo nó "lula"
```
MATCH(lula:Client {name: "Lula Molusco"}) DELETE lula
```

### > Add hobbie no Patrick
```
MATCH(patrick:Client {name:"Patrick"}) SET patrick.hobbies = ['Caçar agua viva']
```
> SET para configurar o nó

### > Alterar a label do Patrick
```
MATCH(patrick:Client {name:"Patrick"}) SET patrick:Client_Premium
```