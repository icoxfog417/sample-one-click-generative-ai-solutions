# Kiro IDE Remote

[Kiro](https://kiro.dev/) は、仕様定義から始まる開発工程全体を支援する Enterprise Ready なアプリケーション構築に適した統合開発環境です。公式サイトからインストールしてご利用いただくことももちろん可能ですが、Kiro IDE Remote ではブラウザ経由でリモートデスクトップに構築された Kiro IDE にアクセスできます。Kiro 以外に、Kiro CLI、AWS CLI、AWS SAM CLI などの開発ツールがプリインストールされておりすぐに開発を始めることができます。

![overview](../assets/images/solutions/kiro-ide/kiro-ide-remote.png)

## 主な特徴

- **クラウドベースの開発環境**: ブラウザからアクセス可能なリモートデスクトップ環境
- **Amazon DCV による高速接続**: 低遅延で快適な開発体験を提供
- **プリインストールされた開発ツール**: Kiro CLI、AWS CLI、AWS SAM CLI、uv、NVM などが利用可能
- **日本語対応**: OS 及び日本語入力をあらかじめセットアップ
- **セキュアなアクセス**: CloudFront と ALB を経由した安全な接続

## AWS へのデプロイ

次のボタンからデプロイできます。AWS へログイン後クリックしてください。

<div class="solution-card__actions">
  <div class="solution-card__deployment">
    <select class="region-selector">
      <option value="ap-northeast-1">東京</option>
      <option value="us-east-1">バージニア</option>
      <option value="us-west-2">オレゴン</option>
    </select>
    <a href="https://ap-northeast-1.console.aws.amazon.com/cloudformation/home#/stacks/create/review?stackName=KiroIDEDeploymentStack&templateURL=https://aws-ml-jp.s3.ap-northeast-1.amazonaws.com/asset-deployments/KiroIDEDeploymentStack.yaml&amp;param_Language=JP" class="deployment-button md-button" target="_blank">
      <i class="fa-solid fa-rocket"></i>　Deploy
    </a>
  </div>
</div>

### パラメータ設定

* UserEmail
    * ユーザーのメールアドレスです。通知の送信先およびシステム設定に使用されます。
* UserFullName
    * ユーザーのフルネームです。Git の設定などに使用されます（デフォルト: Kiro IDE Developer）。
* InstanceType
    * EC2 インスタンスタイプです（デフォルト: t3.xlarge）。Kiro IDE の安定した動作のために十分な CPU リソースを確保しています。
* InstanceVolumeSize
    * EBS ボリュームサイズ（GB）です（デフォルト: 40）。
* RepoUrl
    * 開発用に自動的にクローンする Git リポジトリの URL です（オプション）。
* Language
    * OS の言語設定です。EN（英語）または JP（日本語）を選択できます（デフォルト: EN）。

デプロイが開始すると `UserEmail` に設定したメールアドレスに通知のサブスクリプションを有効化するためのメールが届きます。メールからサブスクライブを行い通知を受け取ってください。

## デプロイ後のアクセス

デプロイが完了すると、下記の案内が書かれたメールが届きます。または、CloudFormation の Outputs タブからも確認できます。

- **KiroIDEURL**: Kiro IDE へのアクセス URL
- **Username**: ログイン用のユーザー名
- **Password**: ログイン用のパスワード
- **InstanceId**: EC2 インスタンス ID

URL にアクセスし、表示されたユーザー名とパスワードでログインしてください。

### 初期設定

* **パスワードはログイン後変更を推奨します**。`passwd` コマンドで変更できます
* デスクトップアイコンの Kiro は最初無効化されています。右クリックで起動を許可してください

![desktop](../assets/images/solutions/kiro-ide/kiro-desktop.png)

### 日本語入力

Ctrl + Space で直接 / 日本語を切り替えられますが、半角・全角キーでの切り替えを行いたい場合一度設定を再起動してください。

![japanese](../assets/images/solutions/kiro-ide/kiro-japanese.png)

### その他

* Kiro CLI で認証がなかなか進まない場合、`kiro-cli login --use-device-flow` を試してみてください
* ターミナルへの Copy & Paste は **Ctrl + Shift + V** になります。これは Linux の仕様です

## 料金について

Kiro IDE Remote のデプロイにかかる主な料金は EC2 インスタンスの費用です。デフォルトの `t3.xlarge` インスタンスを使用した場合の月額料金の目安は以下の通りです（オンデマンド価格、2024年12月時点）：

### 24時間365日稼働の場合（月730時間）

- **東京リージョン (ap-northeast-1)**: 約 $156.85/月（約23,500円/月）
- **バージニアリージョン (us-east-1)**: 約 $121.47/月（約18,200円/月）
- **オレゴンリージョン (us-west-2)**: 約 $121.47/月（約18,200円/月）

### コスト最適化の推奨事項

**使用していない時間はインスタンスを停止することで大幅にコストを削減できます。**

例：平日のみ 1日8時間使用する場合（月160時間）

- **東京リージョン**: 約 $34.82/月（約5,200円/月）
- **米国リージョン**: 約 $26.62/月（約4,000円/月）

インスタンスの停止・起動は以下のコマンドで行えます：

```bash
# インスタンスの停止
aws ec2 stop-instances --instance-ids <InstanceId>

# インスタンスの起動
aws ec2 start-instances --instance-ids <InstanceId>
```

その他の費用として、CloudFront、ALB、EBS、データ転送などの料金が発生しますが、通常の開発用途では月数ドル程度です。

**注意**: 料金は変動する可能性があります。最新の料金は [EC2 料金ページ](https://aws.amazon.com/jp/ec2/pricing/on-demand/) でご確認ください。

## 関連リンク

- [Kiro 公式サイト](https://kiro.dev/)
- [Amazon DCV](https://aws.amazon.com/jp/hpc/dcv/)
