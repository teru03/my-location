# AWS上でのデプロイメントガイド

このドキュメントでは、このアプリケーションをAWS上にデプロイする手順を説明します。

## 1. Amazon EC2インスタンスの準備

1. AWSマネジメントコンソールにログインし、EC2ダッシュボードに移動します
2. 「インスタンスを起動」をクリックします
3. Amazon Linux 2 または Ubuntu Server LTSを選択します
4. 適切なインスタンスタイプを選択します（t2.microから始めることをお勧めします）
5. セキュリティグループで以下のポートを開放します：
   - HTTP (80)
   - HTTPS (443)
   - SSH (22)

## 2. Webサーバーのセットアップ

```bash
# Nginxのインストール
sudo apt-get update
sudo apt-get install nginx

# 必要なファイルをサーバーに転送
scp -i your-key.pem index.html ec2-user@your-instance:/var/www/html/
```

## 3. HTTPSの設定

1. SSL証明書の取得
   - AWS Certificate Manager (ACM)を使用して証明書を取得
   - または、Let's Encryptを使用して無料の証明書を取得

2. ドメインの設定
   - Route 53でドメインを設定
   - Aレコードを作成してEC2インスタンスにマッピング

## 4. セキュリティとスケーリングの考慮事項

- セキュリティグループの適切な設定
- ELB (Elastic Load Balancer)の使用を検討
- Auto Scalingの設定（必要に応じて）
- CloudWatchによるモニタリング設定

## 5. バックアップと保守

- AMI (Amazon Machine Image)を定期的に作成
- EBSスナップショットの定期的な取得
- セキュリティアップデートの定期的な適用

## トラブルシューティング

問題が発生した場合は、以下を確認してください：
- セキュリティグループの設定
- インスタンスのステータスチェック
- Nginxのエラーログ (/var/log/nginx/error.log)
- システムログ (/var/log/syslog)

## コスト最適化

- 必要に応じたインスタンスタイプの選択
- 予約インスタンスの検討
- 使用していないリソースの定期的な確認と削除