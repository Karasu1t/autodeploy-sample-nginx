## 概要

Argo CD を使用して、GitHub 上で管理しているマニフェストに更新が入った際に、自動で Kubernetes クラスターへデプロイを行えるようにする。

## 目的

- Kubernetes マニフェストファイルを作成・管理できるようにする
- Argo CD を実際に触ってみる
- 毎回 `kubectl apply` を実行する煩わしさを解消する

## サブ目的

- Ingress ではなく Gateway API 経由でサーバーに接続できるようにする
- 接続時に自己署名 SSL 証明書を使ったセキュアな通信を実現する
- 複数の Kubernetes クラスター環境で構築・検証を行う
  - `docker-desktop`: Argo CD 用
  - `minikube`: デプロイ対象用

## アーキテクチャ図

![アーキテクチャ図](picture/argocd.png)

## 前提条件

GitHub 上に以下のリポジトリを作成しておく：

- [autodeploy-sample-nginx](https://github.com/Karasu1t/autodeploy-sample-nginx)

## フェーズ構成

本環境構築は以下のフェーズに分けて進める：

1. **Argo CD を Helm を使ってデプロイできること**
2. **自動デプロイ対象のマニフェストファイルを作成し、in-cluster 接続確認ができること**
3. **in-cluster 状態で Argo CD から自動デプロイができること**
4. **外部クラスター（minikube）上で Argo CD から自動デプロイできること**
5. **nginx の index.html のソースコードを更新して自動デプロイできること**
6. **configmap を自動生成できること**

---

各フェーズの詳細手順や設定内容については、以降のセクションに記載。

[Phase 1 - Argo CD を Helm でデプロイする](https://github.com/Karasu1t/autodeploy-sample-nginx/blob/main/Phase1.md)  
[Phase 2 - マニフェストファイルを作成する](https://github.com/Karasu1t/autodeploy-sample-nginx/blob/main/Phase2.md)  
[Phase 3 - in-cluster 状態で Argo CD から自動デプロイする](https://github.com/Karasu1t/autodeploy-sample-nginx/blob/main/Phase3.md)  
[Phase 4 - minikube 上に Argo CD から自動デプロイする](https://github.com/Karasu1t/autodeploy-sample-nginx/blob/main/Phase4.md)
[Phase 5 - nginx の index.html を更新し、自動デプロイする](https://github.com/Karasu1t/autodeploy-sample-nginx/blob/main/Phase5.md)
[Phase 6 - configmap を自動生成する](https://github.com/Karasu1t/autodeploy-sample-nginx/blob/main/Phase6.md)
