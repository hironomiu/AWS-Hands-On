# Step-1

Step-1 では VPC を作成し、Wordpress がインストールされた AMI を用いて EC2 インスタンスを起動します。

## 概要図

![step-1](./images/step-1/step-1.png "STEP1")

---

## Question VPC とは

これから作成する Amazon VPC(Amazon Virtual Private Cloud)について調べてみましょう(10 分)

[公式 Amazon Virtual Private Cloud](https://aws.amazon.com/jp/vpc/)

## Question アベイラビリティゾーンとは

アベイラビリティゾーンについて調べてみましょう(5 分)

## VPC の作成

**実際に VPC を作成してみましょう。まずはサービスタブを選択し VPC 管理ページを開きましょう**

![vpc-1](./images/step-1/vpc-1.png "VPC1")

---

**下にスクロールし VPC を選択します(もしくは上部のテキストボックスで「VPC」と入力し検索しましょう**

![vpc-2](./images/step-1/vpc-2.png "VPC2")

---

**VPC ウィザードの起動を押下します**

![vpc-3](./images/step-1/vpc-3.png "VPC3")

---

**「ステップ 1:VPC 設定の選択」では「1 個のパブリックサブネットを持つ VPC」タブから選択ボタンを押下**

![vpc-4](./images/step-1/vpc-4.png "VPC4")

---

**「ステップ 2:1 個のパブリックサブネットを持つ VPC」では以下を入力し VPC の作成ボタンを押下**

**VPC 名: vpc-ユーザ名(例 vpc-user05 )**  
**アベイラビリティゾーン: ap-northeast-1d**

![vpc-5](./images/step-1/vpc-5.png "VPC5")

---

**「VPC が正常に作成されました」では OK ボタンを押下**

![vpc-6](./images/step-1/vpc-6.png "VPC6")

---

**「VPC ダッシュボード」では VPC の作成ボタン直下のテキスト検索フォームでユーザ名を入力しフィルタリングしましょう。以下の例では user05 でフィルタリングしています**

![vpc-7](./images/step-1/vpc-7.png "VPC7")

---

**作成した VPC の設定が正しいか VPC タブをクリックし内容を確認しましょう**

**名前: vpc-ユーザ名**  
**IPv4 CIDR: 10.0.0.0/16**

![vpc-8](./images/step-1/vpc-8.png "VPC8")

---

**ウィザードで作成したサブネットを確認しましょう。VPC 作成ウィザードでは、VPC 自体と一緒に 1 つ目のサブネットも作成されます。**

![vpc-9](./images/step-1/vpc-9.png "VPC9")

---

**作成したサブネットの Route Table を確認しましょう。VPC のネットワークアドレス 10.0.0.0/16 のターゲットが local に、デフォルトルートの 0.0.0.0/0 のターゲットがインタネットゲートウェイ(igw-XXXX)になっており、インターネットと通信できる設定になっています。**

![vpc-10](./images/step-1/vpc-10.png "VPC10")

---

## サブネットの追加作成

**作成した VPC に対して追加でサブネットを加えましょう。ここではサブネットの 3 つ作成を行いましょう(サブネットを作成を押下)**

![subnet-1](./images/step-1/subnet-1.png "SUBNET1")

---

**作成した VPC を選択しましょう(選択すると下に入力項目が展開されます)**

![subnet-2](./images/step-1/subnet-2.png "SUBNET2")

---

**1 から 3 を以下を参考に設定しましょう。1 つ設定したら ④ の「新しいサブネットを追加」を押下し最後まで設定しましょう。全て設定したら ⑤ の「サブネットを作成」を押下**

| -      | 1 サブネット名               | 2 アベイラビリティ ゾーン | 3 IPv4 CIDR ブロック |
| :----- | :--------------------------- | :------------------------------ | :--------------------- |
| 2 つ目 | パブリックサブネット   | ap-northeast-1c                 | 10.0.1.0/24            |
| 3 つ目 | プライベートサブネット | ap-northeast-1d                 | 10.0.2.0/24            |
| 4 つ目 | プライベートサブネット | ap-northeast-1c                 | 10.0.3.0/24            |

![subnet-2b](./images/step-1/subnet-2b.png "SUBNET2B")

---

**全てのサブネットを確認しましょう。4 つ作成され赤枠の内容が設定通りか確認しましょう。その際に IPv4 CIDR でソートすると見易いです。**

![subnet-3](./images/step-1/subnet-3.png "SUBNET3")

---

**パブリックサブネット(10.0.1.0/24)のルートテーブルを変更しましょう。パブリックサブネット(10.0.1.0/24)を選択しルートテーブルタブを選択、「ルートテーブルの関連付けの編集」ボタンを押下**

![subnet-4](./images/step-1/subnet-4.png "SUBNET4")

---

**現在使用しているルートテーブル以外にもう一つ選択できるはずです。そちらを選択し保存ボタンにて保存しましょう。選択するとインターネットゲートウェイの設定が追加されます。**

![subnet-5](./images/step-1/subnet-5.png "SUBNET5")

---

## EC2 インスタンスの作成

**ここでは 10.0.0.0 のパブリックサブネット内に EC2 インスタンスを作成します。サービスタブから EC2 を選択**

![ec2-1](./images/step-1/ec2-1.png "EC21")

---

**Web サーバの作成を行いましょう。インスタンスを起動プルダウンからインスタンスを起動を押下**

![ec2-2](./images/step-1/ec2-2.png "EC22")

---

**マイ AMI タブ、「1Day-AMI」の選択ボタンを押下**

![ec2-3](./images/step-1/ec2-3.png "EC23")

---

**t2.micro を選択、次の手順：インスタンスの詳細の設定を押下**

![ec2-4](./images/step-1/ec2-4.png "EC24")

---

**ネットワークには自分が作成した VPC を選択、サブネットは 10.0.0.0/24(ap-noatheast-1d)のパブリックサブネットを選択、自動割り当てパブリック IP は「有効」を選択し、次の手順：ストレージの選択を押下**

![ec2-5](./images/step-1/ec2-5.png "EC25")

---

**ストレージの選択では特に何もせず、次の手順：タグの追加を押下**

![ec2-6](./images/step-1/ec2-6.png "EC26")

---

**タグの追加を押下**

![ec2-7](./images/step-1/ec2-7.png "EC27")

---

**キーに「Name」、値に「webserver#1-ユーザ名 例 webserver#1-user05 」を設定し次の手順：セキュリティグループの設定を押下**

![ec2-8](./images/step-1/ec2-8.png "EC28")

---

**新しいセキュリティグループを作成するを選択、セキュリティグループ名は「web-ユーザ名 例 web-user05」を設定、説明も「web-ユーザ名 例 web-user05」を設定し、ルールの追加ボタンを押下**

![ec2-9](./images/step-1/ec2-9.png "EC29")

---

**タイプは HTTP を選択、送信元は任意の場所を選択し確認と作成のボタンを押下**

![ec2-10](./images/step-1/ec2-10.png "EC210")

---

**画面下にスクロールさせ作成(現在は「起動」)を押下**

![ec2-11](./images/step-1/ec2-11.png "EC211")

---

**EC2 サーバに SSH 接続する際に使用する秘密鍵を取得します。新しいキーペアの作成を選択、キーペア名は「1day-ユーザ名 例 1day-user05」を設定し、キーペアのダウンロードを押下、ダウンロードが成功したらインスタンスの作成ボタンを押下**

![ec2-12](./images/step-1/ec2-12.png "EC212")

---

**EC2 インスタンスが作成開始されました。インスタンスの表示ボタンを押下**

![ec2-13](./images/step-1/ec2-13.png "EC213")

---

**作成した EC2 インスタンスにて赤枠で囲った「パブリック DNS(IPv4)」の値をクリップボードにコピーしましょう。サーバログインのドメイン、サンプルアプリケーションである Wordpress の URL となります。**

![ec2-14](./images/step-1/ec2-14.png "EC214")

**「パブリック DNS(IPv4)」の値をブラウザで表示させましょう。Wordpress の設定画面が表示されれば成功です。**

## ![wordpress-1](./images/step-1/wordpress-1.png "Wordpress1")

## Question AMI とは

今回 AMI を使用して EC2 インスタンスを作成しました。この AMI について機能、役割などについて調べてみましょう(10 分)

## サーバログイン

**作成した EC2 インスタンスにログインし環境の確認をしましょう。`1day-userXX.pem`は各自が作成しダウンロードしたた秘密鍵です。ec2-XXXXXX.com は作成した EC2 サーバの「パブリック DNS(IPv4)」の値です。以下はダウンロードした秘密鍵のディレクトリで作業をしています。ssh コマンドで「ec2-user@ip-10-0-0-XX」のプロンプトが返れば成功です**

```
$ chmod 600 1day-userXX.pem
$ ssh -i 1day-userXX.pem -o StrictHostKeyChecking=no ec2-user@ec2-XXXXXX.com
[ec2-user@ip-10-0-0-65 ~]$
```

**MySQL に root ユーザで接続し、所有しているデータベースの確認をしてみましょう**

**パスワードは`wordpress`**

```
$ mysql -u root -p
Enter password:

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| test               |
| wordpress          |
+--------------------+
5 rows in set (0.00 sec)
```

## Question サーバチェック

1.このサーバに振られた IP アドレスの確認をしましょう。  
2.このサーバのデフォルトゲートウェイを確認しましょう。  
3.このサーバの基本スペック(CPU 数、メモリ、DISK 容量など)を確認しましょう。  
4.このサーバで動いている Web サーバがポート 80 を LISTEN しているか確認しましょう。  
5.このサーバで動いている DB サーバがポート 3306 を LISTEN しているか確認しましょう。

## Wordpress の初期設定

**Wordpress の初期設定では「日本語」を選択し続けるボタンを押下**

![wordpress-1](./images/step-1/wordpress-1.png "Wordpress1")

---

**「さあ、始めましょう」を押下**

![wordpress-2](./images/step-1/wordpress-2.png "Wordpress2")

---

**データベース名、ユーザ名、パスワードを以下の設定値にしましょう。その他の値はデフォルトのままで問題ありません。設定をしたら送信ボタンを押下**

| 項目                   | 設定値    |
| :--------------------- | :-------- |
| データベース名         | wordpress |
| ユーザ名               | admin     |
| パスワード             | wordpress |
| データベースのホスト名 | localhost |
| テーブル接頭辞         | wp\_      |

![wordpress-3](./images/step-1/wordpress-3.png "Wordpress3")

---

**インストール実行を押下**

![wordpress-4](./images/step-1/wordpress-4.png "Wordpress4")

---

**サイトのタイトル、ユーザ名、パスワード、メールアドレスを設定しましょう。パスワードは生成された値をメモなどに残しましょう。検索エンジンでの表示はチェックをしインデックスされない設定を有効にしましょう。設定したら WordPress を押下**

![wordpress-5](./images/step-1/wordpress-5.png "Wordpress5")

---

**成功したらログインを押下**

![wordpress-6](./images/step-1/wordpress-6.png "Wordpress6")

---

**ここまでの設定が間違いないか、ユーザ、パスワードを設定しログインしましょう**

![wordpress-7](./images/step-1/wordpress-7.png "Wordpress7")

---

**管理画面にログインできれば設定完了です**

![wordpress-8](./images/step-1/wordpress-8.png "Wordpress8")

---

**「パブリック DNS(IPv4)」の値でブラウザを開きましょう。Wordpress のサイトが開けば設定完了です(表示される内容はバージョンで設定されるデフォルトのテーマにより異なります**

![wordpress-9](./images/step-1/wordpress-9.png "Wordpress9")

---

**ここまでのオペレーションで Step1 は完了です！**
