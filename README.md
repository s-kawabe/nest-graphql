udemy: https://www.udemy.com/share/108fp43@wD4vjdVtuFA9qRKBsl6ZkXrHBA6L4egbulCUvsHHOIlrrKkWfr6q_1OAP0I7rB6Hqg==/

# GraphQL

## スカラー型

1. Int: 整数型
2. Float: 浮動小数点数型
3. String: 文字列型
4. Boolean: 論理型
5. ID: ユニークな文字列型

## ListとNullable

* `!`がついていたらNot Null

```ts
type User {
  tasks: [Task]!
}
```

|  定義  |  意味  |
| ---- | ---- |
|  [String]!  |  リスト自体が必ず存在するがnull要素を持つ可能性がある  |
|  [String!]  |  中身の要素は必ず１つ以上存在するが、リスト自体がnullになる可能性がある  |
|  [String!]!  |  リストが必ず存在し、中身も必ず１つ以上存在する  |
|  [String]  |  リスト自体も中身もnullになる可能性がある  |


## EnumとUnion

```ts
enum Color {
  RED
  BLUE
  GREEN
}
```

```ts
union Vehicle = Car | Bike
```

## Query型

```ts
type Query = {
  getTasks(): [Task]!
  getTaskById(id: Int!): Task
}
```

## Mutation型

```ts
type Mutation = {
  createTask(id: Int!, name: String!): Task!
  deleteTask(id: Int!): Task!
}
```

## コードファーストとスキーマファースト

- コードファースト
  - Nestでは、TypeScriptのコードに特定のでコレーたを付与しGraphQLスキーマを自動生成
  - フロントではバックエンドのコードを基に生成されたスキーマを参照
- スキーマファースト

# NestJS

https://docs.nestjs.com/

- Angularにインスパイアされている
- ExpressをベースとしているのでExpressで出来ることはNestでも出来る
  - Expressの機能やライブラリを利用することができる
  - optionでfastifyをベースにすることも可能
- 拡張性が高い
- nest cliが便利
- Express, FastifyでのネイティブAPIをそのまま使用できる

## Why need Nest ?

Nodeにおける便利なライブラリが日々誕生しているものの、アーキテクチャを標準化するという
問題を解決するものはなかった、そこをNestが担う。


## アーキテクチャ

- ルートはmain.tsの`app.module.ts`
  - これをルートモジュールと呼ぶ
- その下にさまざまな`xxx.module.ts`がある
  -  それごとに、`xxx.service.ts`, `xxx.resolver.ts`が出来上がる
- Dependency Injectionを簡単に出来る仕組みがある → `DIコンテナ`

1. ModuleのProviderに依存される側のクラスを登録する
2. 依存する側のConstructorで注入される側のクラスを引数として受け取る


### Module
- Resolver, Serviceをまとめ、アプリケーションとして利用できるようNestに登録
- 必ず一つのルートモジュールが必要
- classに`@Module`でコレーたをつけて定義する

```ts
@Module({
  imports: [PrismaModule],
  providers: [TaskResolver, TaskService]
})
export class TaskModule {}
```

### Controller (Resolver)
- GraphQLスキーマに定義されたデータを返却する
- REST API(MVC)におけるControllerと似た立ち位置
- @Resolverデコレータを付与、さらに中のメソッドにも@Queryと@Mutationデコレータを付与

```ts
@Resolver()
export class TaskResolver {
  @Query(() => [Task])
  getTasks() {
    // ...
  }

  @Mutation(() => Task)
  createTask() {
    //..
  }
}
```
### Service

- アプリケーション固有のビジネスロジックを定義する
- リゾルバーに書いても動作はするが、責務を分けることで保守性が上がる

```ts
// @Service ではない
@Injectable()
export class UserService {
  // ビジネスロジックを実現するメソッドの作成
  find(userName: number) {
    // ...
  }
}
```

# instllation

```
nest new backend
cd backend
yarn add @nestjs/graphql @nestjs/apollo @apollo/server graphql
```

