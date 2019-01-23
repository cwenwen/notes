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

