# Swaggerまとめ

---

## 概要

### SwaggerSpec

APIの仕様書。YamlもしくはJSONで記述する。この仕様書をもとにAPIやフロントエンドの開発を進めていく。

### SwaggerEditor

https://swagger.io/tools/swagger-editor

SwaggerSpecを書くのに便利なエディタ。プレビューでSwaggerUIを表示してくれる。

### SwaggerUI

https://swagger.io/tools/swagger-ui

SwaggerSpecで書いた仕様書を元にHTML形式でドキュメント化してくれる。

### SwaggerCodegen

https://swagger.io/tools/swagger-codegen/

SwaggerSpecで書いた定義をもとに、クライアント/サーバコードを自動生成するツール。

### Prism

SwaggerSpecで書いた定義を食わせてモックサーバーを立ててくれるツール。

---

## SwaggerSpec

OpenAPIドキュメント。Yaml(もしくはJSON)でOpenAPIオブジェクトを定義する。実用的な話をすると、こんな説明を聞くより公式のサンプルをコピペすればなんとなくわかるので、必要なところを書き換える感じで誰でも書けます。
一応以下で雑に説明する。詳しくは [ここ](https://github.com/OAI/OpenAPI-Specification/tree/main/versions) からドキュメントを読もう。

### OpenAPIObject

| Field Name   |                                           Type                                            |     | Description                                   |
|--------------|:-----------------------------------------------------------------------------------------:|:---:|-----------------------------------------------|
| openapi      |                                         `string`                                          |  *  | OpenAPIのバージョン                                 |
| info         |                                 [InfoObject](#InfoObject)                                 |  *  | メタデータ                                         |
| servers      |                              [[ServerObject](#ServerObject)]                              |     | ターゲットホストのURL                                  |
| paths        |                                [PathsObject](#PathsObject)                                |  *  | パスとリクエストパラメータとレスポンスパラメータの定義                   |
| components   |                           [ComponentsObject](#ComponentsObject)                           |     | 何度も使うオブジェクトの切り出し                              |
| security     |                 [[SecurityRequirementObject](#SecurityRequirementObject)]                 |     | API全体で使用するセキュリティ機構                            |
| tags         |                                  [TagObject](#TagObject)                                  |     | メタデータを含むタグのリスト                                |
| externalDocs |                [ExternalDocumentationObject](#ExternalDocumentationObject)                |     | 追加のメタデータ                                      |
  
3.1.0からwebhooksフィールドが追加され、pathsフィールドが必須ではなくなるらしい。よく知らないので割愛。

### InfoObject

| Field Name     |              Type               |     | Description         |
|----------------|:-------------------------------:|:---:|---------------------|
| title          |            `string`             |  *  | APIのタイトル            |
| summary        |            `string`             |     | APIの概要              |
| description    |            `string`             |     | APIの説明文             |
| termsOfService |            `string`             |     | API利用規約のURL         |
| contact        | [ContactObject](#ContactObject) |     | APIに関する連絡先          |
| license        | [LicenseObject](#LicenseObject) |     | APIのライセンス           |
| version        |            `string`             |  *  | このAPIドキュメント自体のバージョン |

### ContactObject

| Field Name |   Type   |     | Description   |
|------------|:--------:|:---:|---------------|
| name       | `string` |     | 開発者名や企業名      |
| url        | `string` |     | 連絡先となるURL     |
| email      | `string` |     | 連絡先となるメールアドレス |

### LicenseObject

| Field Name |   Type   |     | Description                                                                                 |
|------------|:--------:|:---:|---------------------------------------------------------------------------------------------|
| name       | `string` |  *  | ライセンス名                                                                                      |
| identifier | `string` |     | [SPDX](https://spdx.org/spdx-specification-21-web-version#h.jxpfx0ykyb60) によるライセンス表現、urlと排他 |
| url        | `string` |     | ライセンスのURL、identifierと排他                                                                     |

### ServerObject

| Field Name  |                             Type                             |     | Description                    |
|-------------|:------------------------------------------------------------:|:---:|--------------------------------|
| url         |                           `string`                           |  *  | ターゲットホストのURL                   |
| description |                           `string`                           |     | ターゲットホストの説明文                   |
| variables   | Map[`string`, [ServerVariableObject](#ServerVariableObject)] |     | urlフィールドに変数を利用する場合の、変数とその値の対応表 |

### ServerVariableObject

| Field Name  |    Type    |     | Description                    |
|-------------|:----------:|:---:|--------------------------------|
| enum        | [`string`] |     | 変数の値が限定的である場合のその値の配列、空の配列は指定不可 |
| default     |  `string`  |  *  | 値が指定されなかった場合のデフォルト値            |
| description |  `string`  |     | 変数の説明文                         |

### PathsObject

| Field Pattern |               Type                | Description           |
|---------------|:---------------------------------:|-----------------------|
| /{path}       | [PathItemObject](#PathItemObject) | `/`から始まるエンドポイントへの相対パス |

### PathItemObject

| Field Name  |                                     Type                                     |     | Description                        |
|-------------|:----------------------------------------------------------------------------:|:---:|------------------------------------|
| $ref        |                                   `string`                                   |     | componentsに定義したPathItemObjectを参照する |
| summary     |                                   `string`                                   |     | パスの概要                              |
| description |                                   `string`                                   |     | パスの説明文                             |
| get         |                     [OperationObject](#OperationObject)                      |     | GET操作の定義                           |
| put         |                     [OperationObject](#OperationObject)                      |     | PUT操作の定義                           |
| post        |                     [OperationObject](#OperationObject)                      |     | POST操作の定義                          |
| delete      |                     [OperationObject](#OperationObject)                      |     | DELETE操作の定義                        |
| options     |                     [OperationObject](#OperationObject)                      |     | OPTIONS操作の定義                       |
| head        |                     [OperationObject](#OperationObject)                      |     | HEAD操作の定義                          |
| patch       |                     [OperationObject](#OperationObject)                      |     | PATCH操作の定義                         |
| trace       |                     [OperationObject](#OperationObject)                      |     | TRACE操作の定義                         |
| servers     |                       [[ServerObject](#ServerObject)]                        |     | このパスを処理するターゲットホストのURL              |
| parameters  | [[ParameterObject](#ParameterObject) or [ReferenceObject](#ReferenceObject)] |     | 全ての操作に適用されるパラメータのリスト               |

### OperationObject

| Field Name   |                                           Type                                            |     | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|--------------|:-----------------------------------------------------------------------------------------:|:---:|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| tags         |                                        [`string`]                                         |     | A list of tags for API documentation control. Tags can be used for logical grouping of operations by resources or any other qualifier.                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| summary      |                                         `string`                                          |     | A short summary of what the operation does.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| description  |                                         `string`                                          |     | A verbose explanation of the operation behavior. [CommonMark syntax](https://spec.commonmark.org/) MAY be used for rich text representation.                                                                                                                                                                                                                                                                                                                                                                                                                                                                |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| externalDocs |               [External Documentation Object](#externalDocumentationObject)               |     | Additional external documentation for this operation.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| operationId  |                                         `string`                                          |     | Unique string used to identify the operation. The id MUST be unique among all operations described in the API. The operationId value is **case-sensitive**. Tools and libraries MAY use the operationId to uniquely identify an operation, therefore, it is RECOMMENDED to follow common programming naming conventions.                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| parameters   |      [[Parameter Object](#parameterObject) or [Reference Object](#referenceObject)]       |     | A list of parameters that are applicable for this operation. If a parameter is already defined at the [Path Item](#pathItemParameters), the new definition will override it but can never remove it. The list MUST NOT include duplicated parameters. A unique parameter is defined by a combination of a [name](#parameterName) and [location](#parameterIn). The list can use the [Reference Object](#referenceObject) to link to parameters that are defined at the [OpenAPI Object's components/parameters](#componentsParameters).                                                                     |
| requestBody  |     [Request Body Object](#requestBodyObject) or [Reference Object](#referenceObject)     |     | The request body applicable for this operation.  The `requestBody` is fully supported in HTTP methods where the HTTP 1.1 specification [RFC7231](https://tools.ietf.org/html/rfc7231#section-4.3.1) has explicitly defined semantics for request bodies.  In other cases where the HTTP spec is vague (such as [GET](https://tools.ietf.org/html/rfc7231#section-4.3.1), [HEAD](https://tools.ietf.org/html/rfc7231#section-4.3.2) and [DELETE](https://tools.ietf.org/html/rfc7231#section-4.3.5)), `requestBody` is permitted but does not have well-defined semantics and SHOULD be avoided if possible. |
| responses    |                           [Responses Object](#responsesObject)                            |     | The list of possible responses as they are returned from executing this operation.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |                                                                                                                                                                                                                                                                                                                          |
| callbacks    | Map[`string`, [Callback Object](#callbackObject) or [Reference Object](#referenceObject)] |     | A map of possible out-of band callbacks related to the parent operation. The key is a unique identifier for the Callback Object. Each value in the map is a [Callback Object](#callbackObject) that describes a request that may be initiated by the API provider and the expected responses.                                                                                                                                                                                                                                                                                                               |
| deprecated   |                                         `boolean`                                         |     | Declares this operation to be deprecated. Consumers SHOULD refrain from usage of the declared operation. Default value is `false`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |                                                                                                                                                                                                                                                                                                                          |
| security     |                [[Security Requirement Object](#securityRequirementObject)]                |     | A declaration of which security mechanisms can be used for this operation. The list of values includes alternative security requirement objects that can be used. Only one of the security requirement objects need to be satisfied to authorize a request. To make security optional, an empty security requirement (`{}`) can be included in the array. This definition overrides any declared top-level [`security`](#oasSecurity). To remove a top-level security declaration, an empty array can be used.                                                                                              |                                                                                                                                                                                                                                                                                                                          |
| servers      |                             [[Server Object](#serverObject)]                              |     | An alternative `server` array to service this operation. If an alternative `server` object is specified at the Path Item Object or Root level, it will be overridden by this value.                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |

### ParameterObject

### ReferenceObject

### ComponentsObject

### SecurityRequirementObject

### TagObject

### ExternalDocumentationObject
