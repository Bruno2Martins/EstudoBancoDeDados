# BD Não Relacional

1998 surgiu o NoSQL

surgiu para suprir as deficiencias dos bancos de dados relacionais

**N**ot
**O**nly
**SQL** - NoSQL

## Diferenças
- Escabilidade
    
    BD relacional: escalam de forma vertical onde infla o seu hardware para suportar as requisiçoes que chegam ate ele

    Não relacional: é horizontal, particionamento de dados (usando o sharding) entre os nós, em determinado horario se houver demanda maoir de usuario, faz upgrade para assim utilizar mais nós, da mesma forma quando não precisar faz um downgrade
- SCHEMA (estrutura)
    
    Relacionais: precisao de estruturas bem moldadas utilizando as
    TABELAS, LINHAS, COLUNAS, PK e FK

    Não relacional: o schema é livre
    mas deixar performático

- Performace

    Relacionais: dependem do seu subsistema de disco para performace

    Não Relacional: depende do seu cluste e da latencia da sua rede para uma melçhor performace

- Transações

    Relacional:
    ACID

    Não Relacional:
    BaSE

# Tipos de bancos NoSQL

Temos bancos baseados a:
- Documentos - como json, xml
- Chave-valor - utilizado mais na forma de cache e parecidos
- Colunas - é o q menos apresentam diferencas ao bd relacionas
- Grafos - utilizado em redes, e detecção de fraudes
### NoSQL - Orientação
- MongoDB - documentos
- Redis - Key-value
- Cassandra - colunas
- Neo4j - grafos

## <a href="GRAFO.md">GRAFOS</a>
## <a href="COLUNA.md">COLUNA</a>
## <a href="KEY-VALUE.md">CHAVE-VALOR</a>
## <a href="DOCUMENTO.md">DOCUMENTO</a>

# Introdução Docker
Garante que de forma facil consiga criar e adm os ambientes de forma isolada garantindo disponibilizar para o user final
## funcionalidade
objetivo que a gente crie, teste e implemente aplicações em ambiente separados da maquina a qual vai sere entregue, por ex a nuvem.
Processo chamado de **Contenerização**

## Docker composer
<a href="https://docs.docker.com/compose/">Docker Compose overview</a>

## Docker hub
o docker ja tem uma nuvem de conteiners ja pré conficurados, funcionalidade chamado de ***Docker hub***



