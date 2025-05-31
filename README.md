# なぜPrismなのか & OpenAPIの課題
OpenAPIはAPI仕様を標準化し、設計・ドキュメント・自動生成など多くのメリットをもたらします。
しかし、OpenAPIドキュメントだけでは「実際にAPIを叩いて動作を確認する」ことはできません。
特に以下の課題があります。
- バックエンド実装前にフロントエンドや他システムの開発・検証が進められない
- API仕様の変更が実装に反映されるまで動作確認ができない
- ドキュメントと実装の乖離が発生しやすい

こうした課題を解決するために、OpenAPIドキュメントから自動で「モックサーバー」を立ち上げられるツールが求められます。
Prismは、OpenAPI仕様からリアルなモックサーバーを即座に立ち上げ、API設計段階から実際の通信を伴う開発・検証を可能にします。

## Prismでできること
- モックサーバーの自動生成
OpenAPI v2/v3ドキュメントから、実際のAPIと同じエンドポイント・バリデーション・レスポンス例を持つHTTPサーバーを即座に起動できます。

- バリデーションプロキシ
実際のAPIサーバーへのリクエスト/レスポンスがOpenAPI仕様に準拠しているかを検証できます。

- 幅広い仕様サポート
OpenAPI v2.0（Swagger）、v3.0、v3.1、Postman Collectionに対応。

- CLI・Docker対応
ローカル環境やCI/CDパイプライン、Dockerコンテナでも簡単に利用可能。

## 実際のUsecase
1. フロントエンド開発の高速化
バックエンド未完成でも、OpenAPIドキュメントさえあればフロントエンド開発・結合テストが進められます。

2. API仕様の早期検証・フィードバック
実装前にAPIの使い勝手やデータ構造を実際に叩いて確認し、仕様の問題点を早期に発見できます。

3. CI/CDでのAPIテスト
Prismのバリデーションプロキシ機能を使い、API実装がOpenAPI仕様に準拠しているか自動テストできます。

4. 内部API・外部連携・モバイルアプリ開発
外部パートナーやモバイルアプリ開発者に、実装前からAPIの動作環境を提供できます。
また、マルチプロダクト体制の場合、内部APIをmockする用途にも使えます。

## 実用例
- `openapitools/openapi-generator-cli`でswagger.ymlからjsonを生成
- Dockerfileに生成したjsonを指定
  ```
    FROM stoplight/prism:4.11.0
    COPY openapi.json /
    CMD ["mock", "-h", "0.0.0.0", "/openapi.json"]
  ```
- docker立ち上げ
  ```
  docker compose up
  ```

## 競合サービスとの比較
[mockoon](https://mockoon.com/)の運用上注意
- Mockoon専用の環境ファイルになるため、元のOpenAPI定義の直接差分取り込みはできない
- 固定値を返すだけのモックで十分であれば機能がTooMuchすぎる
↓
- 既存のOpenAPI定義がない場合
- 頻繁に修正が入ることはないAPIをモックする場合
- 簡単な応答だけでなく、複雑なパターンの応答が必要な場合

=> 新規開発時は良さそう。

## 参考
[【OpenAPI】Prismでモックサーバ作成](https://qiita.com/andynuma/items/bf043b5184d3826d0f92)****
[Mockoonがモックサーバーとして優秀だった](https://zenn.dev/edash_tech_blog/articles/188fef6623eaaa)