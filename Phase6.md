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
なお、後続の手順の設定にて Configmap 生成後の Sync も GithubActions 上で実施する

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

</code></pre>

#### 3. Github の Secret に以下の設定を追加

<pre><code>
ARGOCD_SERVER・・・ArgoCDのサーバ名(or IPアドレス)
ARGOCD_USERNAME・・・ArgoCDのユーザー名
ARGOCD_PASSWORD・・・ArgoCDのユーザーパスワード
</code></pre>

#### 3. 更新したソースコード一式を Github レポジトリに Push

#### 4. GithubActions の Workflow が正常に動作していることを確認

#### 5. configmap.yaml が作成されていることを確認

#### 6. ArgoCD 上で Sync されていることを確認

#### 7. ブラウザ上で Web ページが新しくなっていることを確認
