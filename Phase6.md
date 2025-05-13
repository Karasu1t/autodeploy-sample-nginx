## フェーズ 6：configmap を自動生成し、生成後にデプロイする仕組みを実装する

### 概要

フェーズ 5 にて予め Configmap を生成していたが、index.html に変更があった場合に、  
その内容に configmap を手動で再生成してデプロイするのは煩雑であることから  
ソースコードが Github に push されたことをトリガーに congimap を自動で生成される仕組みを  
GithubActions を利用して実装する

---

### 手順

#### 1. ArgoCD の AutoDeploy を無効化

自動デプロイを有効にしたままだと、GithubActions での Configmap 生成前にデプロイされてしまうため無効化する  
※本来であれば、上記の後に GithubActions にて ArgoCD の sync を実行したいが、argoCD を外部公開しないため GithubActions から  
実行できないことから手動でのデプロイとする

<pre><code>
GitHub のリポジトリにアクセス

「Settings」→「Actions」→「General」

Workflow permissions セクションにて：

✅ Read and write permissions を選択

✅ Save
</code></pre>

#### 2. GithubActions 用の yaml ファイルを作成、および configmap 用の yaml ファイルを削除

tree 上で src フォルダを作成し、その中に差し替え用の index.html ファイルを格納  
※index.html の内容については何でも良いのでコードについては割愛(ChatGTP でおしゃれなものを作成してもらった)

<pre><code>
├── .github
│   └── workflows
│       └── generate-configmap.yaml ※NEW
├── manifest (configmap.yamlを削除)
│   ├── depl.yaml
│   └── service.yaml
└── src
    └── index.html
</code></pre>

◆generate-configmap.yaml

<pre><code>
name: CI for ArgoCD App

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Generate ConfigMap YAML from index.html
        run: |
          kubectl create configmap cm-nginx-index \
            --from-file=index.html=./src/index.html \
            --dry-run=client -o yaml > ./manifest/cm-nginx-index.yaml

      - name: Commit updated manifest
        run: |
          git config --global user.name 'GitHub Actions'
          git config --global user.email 'actions@github.com'
          git add ./manifest/cm-nginx-index.yaml
          git commit -m "Auto-generate ConfigMap from index.html" || echo "No changes to commit"
          git push
</code></pre>

#### 3. 更新したソースコード一式を Github レポジトリに Push

#### 4. GithubActions の Workflow が正常に動作していることを確認

#### 5. configmap.yaml が作成されていることを確認

#### 6. ArgoCD 上で Sync されていることを確認

#### 7. ブラウザ上で Web ページが新しくなっていることを確認
