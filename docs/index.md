# MkDocsで“読まれる社内ナレッジベース”を作ってみた話  
～GitHub Actionsによる自動公開と、ちょっとしたAI活用も添えて～

## はじめに

業務の中で「この知識、チームで共有しておきたいな」と思う瞬間はありませんか？  
ちょっとした手順メモ、表記ルール、よくある質問…。それらがきちんと整理されたナレッジベースになっていれば、誰かの手間や不安を減らすことができるはずです。

とはいえ、「整理するのが面倒」「公開の仕組みがわからない」といった理由で、  
情報が個人メモやローカルファイルにとどまってしまうケースも多いのではないでしょうか。

そんな背景から、**MkDocs + GitHub Pages** を使って、  
**自分でも作れる“読みやすい社内ナレッジベース”**をテーマにサイトを構築してみました。

この記事ではその体験をもとに、

- 構築の流れ（環境・技術選定）
- 実際の情報構成
- AIのちょっと便利な使い方
- つまずいたトラブルとその対応

を紹介します。  
公式ドキュメントを読めば分かることは省略しつつ、**構成の工夫や“どこでつまずいたか”といった実践的な視点**を重視しています。

---

## 構築環境と技術選定

### 使用した構成

- ドキュメント生成：**MkDocs**
- テーマ：**Material for MkDocs**
- ホスティング：**GitHub Pages**
- 自動デプロイ：**GitHub Actions**

### なぜMkDocsを選んだか

#### ✅ Markdownベースで書ける

普段のメモや手順書にMarkdownを使っている人なら、すぐに馴染めます。  
エンジニアだけでなく、非エンジニアにも書きやすいのがメリットです。

#### ✅ ドキュメントに特化している

MkDocsは「構造化されたドキュメントサイト」を作るためのツールです。  
ブログ系のジェネレーターと異なり、**技術文書向けのUIやナビゲーションが標準で備わっている**点が大きな特徴です。

#### ✅ Materialテーマが強力

見た目がモダンで、検索やバージョン切り替えなどの機能も豊富。  
「とりあえず書いたけど見づらい…」という課題が一気に解決しました。

---

### GitHub Pages + Actions の組み合わせ

GitHub Pagesを使えば、`site/` にビルドされた静的ファイルを無料でホスティングできます。  
さらに GitHub Actions を活用することで以下のような自動化が可能になります：

- `main` ブランチに変更をPushすると
- 自動で `mkdocs build` を実行し、`gh-pages` ブランチへデプロイ

つまり、**更新＝公開**がノータイムで完了します。

---

### 導入準備について（簡易メモ）

今回の環境構成は以下のとおりです：

- macOS（Intel）
- Python 3.9 + pip
- GitHub CLI（gh）
- エディタ：VS Code

MkDocsとMaterialテーマの導入は以下のコマンドで行いました：

```bash
pip install mkdocs
pip install mkdocs-material
```

> 詳しくは [公式Getting Started](https://www.mkdocs.org/getting-started/) を参照してください。

---

### メリット・デメリットまとめ

| メリット | 内容 |
|----------|------|
| ✅ 導入が軽い | Pythonさえあれば数分で始められる |
| ✅ UIが整っている | Materialテーマで視認性が大幅に向上 |
| ✅ 無料で公開できる | GitHub Pagesのおかげでホスティングコストはゼロ |
| ✅ 自動化できる | ActionsでCI的な運用が可能 |

| デメリット | 対処ポイント |
|------------|--------------|
| ❌ 初回の構築で詰まりがち | `mkdocs gh-deploy` の役割を事前に理解しておく |
| ❌ YAML構文に厳しい | エディタの構文補助やAIを活用 |
| ❌ デザインのカスタマイズにクセあり | 基本はMaterialテーマで満足できるレベル |

---

## ナレッジベースの構成例

今回のドキュメントサイトは「社内向けナレッジベース」を想定して、以下のような構成で設計しました。

### トップページ：ようこそページ

- プロジェクトの目的や概要
- このドキュメントの使い方
- 全体の構成紹介

「このページが何のためにあるのか」がすぐにわかるように、最初の1ページで全体像がつかめるよう設計しました。

---

### カテゴリ別のセクション構成

- **導入・初期設定**：環境構築やアカウント設定など
- **業務フロー・操作手順**：実際に行う作業をステップ形式で記載
- **FAQ**：よくある質問とその対応
- **表記ルール・用語集**：ドキュメント全体での共通認識を形成

---

### ナビゲーション構成例（`mkdocs.yml`）

```yaml
nav:
  - ようこそ: index.md
  - セットアップ:
      - 環境構築: setup/environment.md
      - アカウント設定: setup/account.md
  - 業務フロー:
      - 操作手順: workflow/steps.md
      - よくある質問: workflow/faq.md
  - ガイドライン:
      - 表記ルール: guideline/style.md
      - 用語集: guideline/terms.md
```

---

## AIでちょっと便利だったこと

ChatGPTなどの生成AIを“便利な補助ツール”として活用することで、作業の効率が少し上がりました。

---

### ✅ 情報構成のブレスト相手として

構成を考えるときに「どんなページを用意すべきか」と相談することで、  
ゼロから考える時間を短縮できました。

---

### ✅ 表現や言い回しの相談相手として

表記の揺れに迷ったときに、複数案を出してもらい、**比較して納得して選ぶ**という判断がしやすくなりました。

---

### ✅ YAMLやMarkdownの構文サンプル生成

インデントが崩れやすいYAMLや、メニュー構成の例なども気軽に確認できます。  
構文ミスで詰まる時間を減らせたのは大きなメリットでした。

---

## トラブルと対応の記録

### ❗ `mkdocs` コマンドが使えない

macOSでは、インストール後にPATHが通っていないことがあります。  
以下を `.zshrc` に追加することで解決しました：

```bash
export PATH=$PATH:/Users/yourname/Library/Python/3.9/bin
```

---

### ❗ ホームディレクトリで `git init` してしまう

ホームで実行すると全ファイルをGitが追跡してしまいます。  
`.git` を削除し、適切なプロジェクトフォルダでやり直しました。

---

### ❗ GitHub Actions が `exit code 128`

`gh-pages` ブランチが存在していなかったため、  
一度 `mkdocs gh-deploy` を手動実行して解決しました。

---

### ❗ `mkdocs.yml` の構文ミス

インデントずれでエラー。エディタのYAMLチェックやAIによる構文補正で修正できました。

---

## おわりに

読みやすいドキュメントをつくるには、構成や用語、トラブル対応に至るまで、細やかな気配りが必要だと実感しました。

今回の体験を通して：

- 情報設計と構成の大切さ
- 小さな工夫が読みやすさに直結すること
- AIや自動化がサポートになること

を実感できました。

この記事が、これからドキュメントを整備しようとしている方や、  
ナレッジベースづくりに取り組む方のヒントになれば嬉しいです。

ご覧いただきありがとうございました。
