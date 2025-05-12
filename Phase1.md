## フェーズ 1：Argo CD を Helm でデプロイする

### 概要

Helm を使用して Argo CD を Kubernetes クラスター上にデプロイし、Web コンソール（UI）にログインできる状態を確認する。

---

### 手順

#### 1. Helm レポジトリの追加と更新

```bash
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
