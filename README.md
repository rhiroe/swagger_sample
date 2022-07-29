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

OpenAPIドキュメント。Yaml(もしくはJSON)でOpenAPIオブジェクトを定義する。

### OpenAPIObject

| Field Name   | Type                                                        |     | Description                                                                                                         |
|--------------|-------------------------------------------------------------|-----|---------------------------------------------------------------------------------------------------------------------|
| openapi      | string                                                      | *   | OpenAPI仕様書のバージョンを指定する。                                                                                              |
| info         | [InfoObject](#InfoObject)                                   | *   | APIに関するメタデータを記述する。                                                                                                  |
| servers      | [[ServerObject](#ServerObject)]                             |     | ターゲットサーバへの接続情報を記述する。未指定の場合、デフォルトでURLが`/`のServerObjectとなる                                                            |
| paths        | [PathsObject](#PathsObject)                                 | *   | APIで利用可能なパスとリクエストや返却するデータの構成の定義。                                                                                    |
| components   | [ComponentsObject](#ComponentsObject)                       |     | 何度も利用するオブジェクトをコンポーネントとしてここに切り出すことができる。                                                                              |
| security     | [[SecurityRequirementObject](#SecurityRequirementObject)]   |     | API全体で使用するセキュリティ機構を指定する。SecurityRequirementObjectのうち1つだけが満たされる場合にリクエストは承認される。上書きが可能で、オプショナルにするためにはからのオブジェクトを配列に含める。 |
| tags         | [TagObject](#TagObject)                                     |     | タグのリストを記述できる。ここには追加のメタデータを指定でき、解析ツール等で利用できる。                                                                        |
| externalDocs | [ExternalDocumentationObject](#ExternalDocumentationObject) |     | 概要を書くところ。                                                                                                           |
  

### InfoObject

### ServerObject

### PathsObject

### ComponentsObject

### SecurityRequirementObject

### TagObject

### ExternalDocumentationObject