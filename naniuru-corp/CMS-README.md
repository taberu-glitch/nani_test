# ナニウル サイト ＋ CMS 連動ガイド

このリポジトリは、コーポレートサイト（静的HTML）と、それを更新する管理画面 `naniuru-cms.html` がセットになっています。
CMSから「買取実績」と「ブログ」を追加すると、サイトの該当ページに自動で反映されます。

## 仕組み（連動ポイント）

CMSはGitHub API経由で、以下のファイルを直接書き換えます。

| CMSのコンテンツ | 生成・更新されるファイル | 連動マーカー |
|---|---|---|
| ブログ記事（本文ページ） | `blog/<slug>.html`（新規作成 / 上書き） | — |
| ブログ一覧 | `blog/index.html` | `<!-- CMS-BLOG-CARDS -->` の直後に新着カードを挿入。`data-blog-id` で重複判定・更新 |
| 買取実績 | `results/index.html` | `</div><!-- /results-grid -->` の直前にカードを挿入。`data-result-id` で重複判定・更新 |
| 画像 | `blog/images/`, `results/images/` | 分離モード時に自動push |

※ これらのマーカー・ID属性はサイト側に既に埋め込み済みです。**消さないでください。**

## 使い方

### A. GitHubに自動公開（推奨）
1. このフォルダ一式をGitHubリポジトリにアップロードし、GitHub Pagesを有効化。
2. ブラウザで `naniuru-cms.html` を開く。
3. 左メニュー「⚙️ GitHub設定」で、Personal Access Token・owner・repo・branch を登録。
   - Tokenは `Contents: Read and write` 権限が必要（Fine-grained token推奨）。
4. ブログ・買取実績を作成。
5. 「🚀 サイトに公開」→「公開」。30〜60秒でサイトに反映されます。

### B. 手動ダウンロードで反映
GitHubを使わない場合は、CMSの「エクスポート」から：
- ブログ本文HTML → `blog/` に配置
- 「📚 ブログ一覧カード」スニペット → `blog/index.html` の `<!-- CMS-BLOG-CARDS -->` 直後に貼り付け
- 「🏆 買取実績」スニペット → `results/index.html` の `</div><!-- /results-grid -->` 直前に貼り付け

## 注意
- CMSのデータはブラウザの localStorage に保存されます。同じブラウザで開いてください。
- 既存の実績カードには `seed-01`〜`seed-12` のIDを付与済み。CMSで管理する新規分は別IDになります。

## サブフォルダ公開について（重要）

リポジトリ直下ではなく `naniuru-corp/` などのサブフォルダにサイトを置いた場合は、
GitHub設定の「サイトのベースパス」に `/naniuru-corp/` と入力してください。
CMSのpush先パスが自動でそのフォルダ配下に補正されます（例：`naniuru-corp/blog/index.html`）。
リポジトリ直下に置く場合は `/`（空欄）のままでOKです。
