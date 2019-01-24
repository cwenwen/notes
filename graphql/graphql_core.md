# [Core Concepts](https://www.howtographql.com/basics/2-core-concepts/)

## The Schema Definition Language (SDL)

GraphQL 使用 type system 來定義 API 的 *schema*。  
這類的語法叫做 Schema Definition Language (SDL)。  

例如定義一個叫做 `Person` 的 type：

```graphql
type Person {
  name: String!
  age: Int!
}
```

`Person` 可以和 `Post` 相關：

```graphql
type Post {
  title: String!
  author: Person!
}
```

在 `Person` 這一端也要放上 `Post` 相關的連結（一 `Person` 對多 `Post`，`Post` 以 array 形式存在 `Person` 中）：

```graphql
 type Person {
  name: String!
  age: Int!
  posts: [Post!]!
}
```

## Fetching Data with Queries

使用 REST APIs 時，資料會來自多個結構清楚且固定的 endpoint，可能會回傳一大包（不一定用得到的）資料。而 GraphQL APIs 只需要一個 endpoint，讓 client 決定回傳的資料結構，只傳回需要的就好。

Clinet 需要用 query 來給 server 更多資訊。

### Basic Queries

假設 data 長這樣：

**Person**

| id | name | age |
| -- | -- | -- |
| cjr8yibv90knp0195n8d5frox | Johnny | 23 |
| cjr8yibwa0knv0195oy964jxq | Sarah | 20 |
| cjr8yibwx0knz01951eon2m2x | Alice | 20 |

**Post**

| id | title |
| -- | -- |
| cjr8yibv90knq0195dcnuphj8 | GraphQL is awesome |
| cjr8yibv90knr0195w135wyh3 | Relay is a powerful GraphQL Client |
| cjr8yibwa0knw0195efuwvt0s | How to get started with React & GraphQL |

Query 範例：

```graphql
{
  allPersons {
    name
  }
}
```

`allPersons` 是這個 query 的 **root field**，在 root field 之下的是這個 quert 的 **payload**。  
以上 query 的 response（只會得到 name，不會有其他欄位）：

```json
{
  "allPersons": [
    { "name": "Johnny" },
    { "name": "Sarah" },
    { "name": "Alice" }
  ]
}
```

若需要 age，就這樣下 query：

```graphql
{
  allPersons {
    name
    age
  }
}
```

如果需要每個 `Person` 的 `Post`：

```graphql
{
  allPersons {
    name
    age
    posts {
      title
    }
  }
}
```

Response 會長這樣：

```json
{
  "data": {
    "allPersons": [
      {
        "name": "Johnny",
        "age": 23,
        "posts": [
          {
            "title": "GraphQL is awesome"
          },
          {
            "title": "Relay is a powerful GraphQL Client"
          }
        ]
      },
      {
        "name": "Sarah",
        "age": 20,
        "posts": [
          {
            "title": "How to get started with React & GraphQL"
          }
        ]
      },
      {
        "name": "Alice",
        "age": 20,
        "posts": []
      }
    ]
  }
}
```

### Queries with Arguments


每個欄位可以有零個或多個 arguments，例如 `last` 這個 argument 可指定特定的回傳數量：

```graphql
{
  allPersons(last: 2) {
    name
  }
}
```

Response：

```json
{
  "data": {
    "allPersons": [
      {
        "name": "Sarah"
      },
      {
        "name": "Alice"
      }
    ]
  }
}
```

## Writing Data with Mutations

使用 mutations 來做這三種改變：

- 新增
- 更新
- 刪除

語法結構都和 queries 一樣，但開頭需要 `mutation` 這個關鍵字：

```graphql
mutation {
  createPerson(name: "Bob", age: 36) {
    name
    age
  }
}
```

這個 mutation 的 root field 是 `createPerson`。  
The server response 長這樣：

```json
"createPerson": {
  "name": "Bob",
  "age": 36,
}
```

資料變這樣：

| id | name | age |
| -- | -- | -- |
| cjr8yibv90knp0195n8d5frox | Johnny | 23 |
| cjr8yibwa0knv0195oy964jxq | Sarah | 20 |
| cjr8yibwx0knz01951eon2m2x | Alice | 20 |
| cjra0ucji19r801974cgd0ntb | Bob | 36 |

如果再這樣寫：

```graphql
mutation {
  createPerson(name: "Alice", age: 36) {
    id
  }
}
```

The server response 長這樣：

```json
{
  "data": {
    "createPerson": {
      "id": "cjra12gf700cg0185h6p53bg4"
    }
  }
}
```

資料變這樣：

| id | name | age |
| -- | -- | -- |
| cjr8yibv90knp0195n8d5frox | Johnny | 23 |
| cjr8yibwa0knv0195oy964jxq | Sarah | 20 |
| cjr8yibwx0knz01951eon2m2x | Alice | 20 |
| cjra0ucji19r801974cgd0ntb | Bob | 36 |
| cjra12gf700cg0185h6p53bg4 | Alice | 36 |

## Realtime Updates with Subscriptions

使用 subscription 和 server 聯繫，取得即時的資料。

Subscriptions 不像 queries 和 mutations 遵循典型的 *“request-response-cycle”*，它的資料是 *stream* 的概念。

假設我們要 subscribe 所有 `Person` type 的 events：

```grapgql
subscription {
  newPerson {
    name
    age
  }
}
```

Subscribe 後，如果有 mutation 建立了新的 `Person`，server 就會回傳資訊給 client：

```json
{
  "newPerson": {
    "name": "Jane",
    "age": 23
  }
}
```

## Defining a Schema

The *schema* 是 GraphQL API 最重要的概念之一，可視為 server 和 client 之間的 contract，定義所有可能用到的 fields 和 arguments。  

Schema 的 *root* types:

```graphql
type Query { ... }
type Mutation { ... }
type Subscription { ... }
```

以上範例用的所有 schema：

```graphql
type Query {
  allPersons(last: Int): [Person!]!
}

type Mutation {
  createPerson(name: String!, age: Int!): Person!
}

type Subscription {
  newPerson: Person!
}

type Person {
  name: String!
  age: Int!
  posts: [Post!]!
}

type Post {
  title: String!
  author: Person!
}
```