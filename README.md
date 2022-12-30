# Docker 環境で Vue の検証環境を構築

## 技術スタック

* Vue.js 3
* Nginx 1.23

## 構築

1. ビルド
   - `docker-compose build`
2. 起動
    - `docker-compose up`

## Docker Hub へ登録

登録済み Docker Hub アカウントへログインしている状態で、イメージを push する

1. イメージプッシュ
   - `docker push ysofficellc/vue3-aws-eks:1.0.0`

## AWS EKS へデプロイ

### クラスター作成

- `eksctl create cluster --name=vue3-kube-cluster --nodes=2 --node-type=t2.micro`
  - 結構時間かかる

### Kubernetes へ kubectl を用いてデプロイする

1. Deployments 投入
   - `kubectl apply -f kubernetes/deployment.yaml`
2. Service 投入
   - `kubectl apply -f kubernetes/service.yaml`
3. 動作確認
   - `kubectl get nodes`
   - `kubectl get pods`
   - `kubectl get services`
     - 参考
     ![image](https://user-images.githubusercontent.com/108514223/210063233-c4711c78-01a4-45b9-9541-fae70e731778.png)

## 参考資料
* [VueアプリケーションをDockerのNginx上で公開する](https://qiita.com/yama9112/items/3cdb4dd3ce718d2f6c4d)
* [Docker Compose - docker-compose.yml リファレンス](https://qiita.com/zembutsu/items/9e9d80e05e36e882caaa)
* [Deploy a Laravel App to Amazon EKS in 5 minutes](https://gbengaoni.com/blog/Deploy-a-Laravel-App-to-Amazon-EKS-in-5-minutes-a94a41436157)
