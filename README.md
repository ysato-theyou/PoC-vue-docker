# Docker 環境で Vue の検証環境を構築

## 技術スタック

* Vue.js 3
* Nginx 1.23

## 構築

1. ビルド
   * `docker build . -t ysofficellc/vue3-aws-eks:1.0.0`

## Docker Hub へ登録

登録済み Docker Hub アカウントへログインしている状態で、イメージを push する

- タグを `{username}/{image}` に変更後に push する
  - `docker push ysofficellc/vue3-aws-eks:1.0.0`

## AWS EKS へデプロイ

1. Vue.js 3 プロジェクトを作成
   - `docker-compose exec app composer create-project --prefer-dist laravel/laravel laravel-aws-eks`
2. 権限修正（環境に依るので本来は環境変数で調整するほうが良い）
   - `sudo chown -R www-data: laravel-aws-eks/`
3. イメージビルド
   - `docker build -f docker/php/apache.Dockerfile laravel-aws-eks -t ysofficellc/laravel-aws-eks:v2`
4. イメージプッシュ
   - `docker push ysofficellc/laravel-aws-eks:v2`

### クラスター作成

- `eksctl create cluster --name=laravel-kube-cluster --nodes=2 --node-type=t2.micro`
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
        ```shell
        ```

## 参考資料
* [VueアプリケーションをDockerのNginx上で公開する](https://qiita.com/yama9112/items/3cdb4dd3ce718d2f6c4d)
* [Deploy a Laravel App to Amazon EKS in 5 minutes](https://gbengaoni.com/blog/Deploy-a-Laravel-App-to-Amazon-EKS-in-5-minutes-a94a41436157)
