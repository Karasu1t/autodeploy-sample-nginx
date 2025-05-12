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

※後で挿入予定

## 前提条件

GitHub 上に以下のリポジトリを作成しておく：

- [autodeploy-sample-nginx](https://github.com/Karasu1t/autodeploy-sample-nginx)

## フェーズ構成

本環境構築は以下のフェーズに分けて進める：

1. **Argo CD を Helm を使ってデプロイできること**
2. **自動デプロイ対象のマニフェストファイルを作成し、in-cluster 接続確認ができること**
3. **in-cluster 状態で Argo CD から自動デプロイができること**
4. **外部クラスター（minikube）上で Argo CD から自動デプロイできること**

---

各フェーズの詳細手順や設定内容については、以降のセクションで記載予定。
