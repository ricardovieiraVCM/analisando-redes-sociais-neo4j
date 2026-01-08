# 📊 Analisando Dados de Redes Sociais com Grafos (Neo4j)

Projeto desenvolvido como parte do desafio da **Digital Innovation One (DIO)**, com foco na análise de dados de redes sociais utilizando **banco de dados orientado a grafos (Neo4j)**.

---

## 🎯 Objetivo do Projeto

Modelar e analisar uma rede social simulada, permitindo identificar conexões entre usuários, relações de seguidores e padrões de interação por meio de consultas Cypher.

---

## 🛠️ Tecnologias Utilizadas

- Neo4j AuraDB
- Linguagem Cypher
- Git & GitHub

---

## 🧩 Modelo de Dados

### Nós
- **User**
  - `id`: identificador único
  - `name`: nome do usuário

### Relacionamentos
- **FOLLOWS**
  - Indica que um usuário segue outro

---

## 📂 Carga de Dados

Os dados foram carregados a partir de arquivos CSV utilizando o comando `LOAD CSV`.

### Usuários
```cypher
LOAD CSV WITH HEADERS FROM 'file:///users.csv' AS row
CREATE (:User {
  id: toInteger(row.id),
  name: row.name
});
LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
MATCH (a:User {id: toInteger(row.from)}),
      (b:User {id: toInteger(row.to)})
CREATE (a)-[:FOLLOWS]->(b);
// Total de usuários
MATCH (u:User)
RETURN count(u);

// Usuários mais seguidos
MATCH (u:User)<-[:FOLLOWS]-(f)
RETURN u.name, count(f) AS seguidores
ORDER BY seguidores DESC;

// Caminhos entre usuários
MATCH p=shortestPath(
  (a:User {name: 'Alice'})-[:FOLLOWS*]-(b:User {name: 'Bob'})
)
RETURN p;
