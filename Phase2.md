## フェーズ 2：マニフェストファイルを作成する

### 概要

ArgoCD にて自動デプロイするオリジナルの Web ページ元の manifest ファイルを作成する  
※Deployment にて Pod を作成する

---

### 手順

#### 1. manifest ファイルの作成

※本レポジトリの manifest フォルダに格納済
└── manifest
├── depl.yaml
└── service.yaml

depl.yaml → Web ページ作成用の Deployment に関する内容を記述  
service.yaml → Pod に接続するための SVC に関する内容を記述

◆depl.yaml

<pre><code>
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: deployment-nginx
  name: deployment-nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: deployment-nginx
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: deployment-nginx
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
status: {}
</code></pre>

<pre><code>
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: deployment-nginx
  name: nginx-service
spec:
  ports:
    - port: 80
      protocol: TCP
      targetPort: 80
      nodePort: 30080
  type: NodePort
  selector:
    app: deployment-nginx
status:
  loadBalancer: {}
</code></pre>
