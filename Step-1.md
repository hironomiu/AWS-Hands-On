# Step-1
Step-1ではVPCを作成し、WordpressがインストールされたAMIを用いてEC2インスタンスを起動します。

## 概要図

![step-1](./images/step-1/step-1.png "STEP1")

----

## Question VPCとは
これから作成するAmazon VPC(Amazon Virtual Private Cloud)について調べてみましょう(10分)

[公式 Amazon Virtual Private Cloud](https://aws.amazon.com/jp/vpc/)

## Question アベイラビリティゾーンとは
アベイラビリティゾーンについて調べてみましょう(5分)

## VPCの作成
**実際にVPCを作成してみましょう。まずはサービスタブを選択しVPC管理ページを開きましょう**

![vpc-1](./images/step-1/vpc-1.png "VPC1")

----

**下にスクロールしVPCを選択します**

![vpc-2](./images/step-1/vpc-2.png "VPC2")

----

**VPCウィザードの開始を選択します**

![vpc-3](./images/step-1/vpc-3.png "VPC3")

----

**「ステップ1:VPC設定の選択」では「1個のパブリックサブネットを持つVPC」タブから選択ボタンを押下**

![vpc-4](./images/step-1/vpc-4.png "VPC4")

----

**「ステップ2:1個のパブリックサブネットを持つVPC」では以下を入力しVPCの作成ボタンを押下**

**VPC名: vpc-ユーザ名(例 vpc-user05 )**  
**アベイラビリティゾーン: ap-northeast-1d**  

![vpc-5](./images/step-1/vpc-5.png "VPC5")

----

**「VPCが正常に作成されました」ではOKボタンを押下**

![vpc-6](./images/step-1/vpc-6.png "VPC6")

----

**「VPCダッシュボード」では直下の「VPCでフィルタリング」でユーザ名を入力しフィルタリングしましょう。以下の例ではuser05でフィルタリングしています**

![vpc-7](./images/step-1/vpc-7.png "VPC7")

----

**作成したVPCの設定が正しいかVPCタブをクリックし内容を確認しましょう**

**名前: vpc-ユーザ名**  
**IPv4 CIDR: 10.0.0.0/16**  

![vpc-8](./images/step-1/vpc-8.png "VPC8")

----

**ウィザードで作成したサブネットを確認しましょう。VPC作成ウィザードでは、VPC自体と一緒に1つ目のサブネットも作成されます。**

![vpc-9](./images/step-1/vpc-9.png "VPC9")

----

**作成したサブネットのRoute Tableを確認しましょう。VPCのネットワークアドレス 10.0.0.0/16 のターゲットがlocalに、デフォルトルートの 0.0.0.0/0 のターゲットがインタネットゲートウェイ(igw-XXXX)になっており、インターネットと通信できる設定になっています。**
 
![vpc-10](./images/step-1/vpc-10.png "VPC10")

----

## サブネットの追加作成

**作成したVPCに対して追加でサブネットを加えましょう。ここではサブネットの3つ作成を行いましょう**

![subnet-1](./images/step-1/subnet-1.png "SUBNET1")

----
**1から4を以下を参考に設定しましょう。**

|-|1 名前タグ|2 VPC|3 アベイラビリティ ゾーン|4 CIDR ブロック|
|:-|:-|:-|:-|:-|
|2つ目|パブリックサブネット|自分で作成したVPCを指定|ap-northeast-1c|10.0.1.0/24|
|3つ目|プライベートサブネット|自分で作成したVPCを指定|ap-northeast-1d|10.0.2.0/24|
|4つ目|プライベートサブネット|自分で作成したVPCを指定|ap-northeast-1c|10.0.3.0/24|

![subnet-2](./images/step-1/subnet-2.png "SUBNET2")

----
**全てのサブネットを確認しましょう。4つ作成され赤枠の内容が設定通りか確認しましょう。その際にIPv4でソートすると見易いです。**

![subnet-3](./images/step-1/subnet-3.png "SUBNET3")

----
**パブリックサブネット(10.0.1.0/24)のルートテーブルを変更しましょう。パブリックサブネット(10.0.1.0/24)を選択しルートテーブルタブを選択、編集ボタンを押下**

![subnet-4](./images/step-1/subnet-4.png "SUBNET4")

----
**現在使用しているルートテーブル以外にもう一つ選択できるはずです。そちらを選択し保存ボタンにて保存しましょう。選択するとインターネットゲートウェイの設定が追加されます。**

![subnet-5](./images/step-1/subnet-5.png "SUBNET5")

----

## EC2インスタンスの作成
**ここでは10.0.0.0のパブリックサブネット内にEC2インスタンスを作成します。サービスタブからEC2を選択**

![ec2-1](./images/step-1/ec2-1.png "EC21")

----
**Webサーバの作成を行いましょう。インスタンスの作成ボタンを押下**

![ec2-2](./images/step-1/ec2-2.png "EC22")

----
**マイAMIタブ、「1Day-AMI」の選択ボタンを押下**

![ec2-3](./images/step-1/ec2-3.png "EC23")

----
**t2.microを選択、次の手順：インスタンスの詳細の設定を押下**

![ec2-4](./images/step-1/ec2-4.png "EC24")

----
**ネットワークには自分が作成したVPCを選択、サブネットは10.0.0.0/24(ap-noatheast-1d)のパブリックサブネットを選択、自動割り当てパブリックIPは有効化を選択し、次の手順：ストレージの選択を押下**

![ec2-5](./images/step-1/ec2-5.png "EC25")

----
**ストレージの選択では特に何もせず、次の手順：タグの追加を押下**

![ec2-6](./images/step-1/ec2-6.png "EC26")

----
**タグの追加を押下**

![ec2-7](./images/step-1/ec2-7.png "EC27")

----
**キーに「Name」、値に「webserver#1-ユーザ名 例 webserver#1-user05 」を設定し次の手順：セキュリティグループの設定を押下**

![ec2-8](./images/step-1/ec2-8.png "EC28")

----
**新しいセキュリティグループを作成するを選択、セキュリティグループ名は「web-ユーザ名 例 web-user05」を設定、説明も「web-ユーザ名 例 web-user05」を設定し、ルールの追加ボタンを押下**

![ec2-9](./images/step-1/ec2-9.png "EC29")

----
**タイプはHTTPを選択、送信元は任意の場所を選択し確認と作成のボタンを押下**

![ec2-10](./images/step-1/ec2-10.png "EC210")

----
**画面下にスクロールさせ作成を押下**

![ec2-11](./images/step-1/ec2-11.png "EC211")

----
**EC2サーバにSSH接続する際に使用する秘密鍵を取得します。新しいキーペアの作成を選択、キーペア名は「1day-ユーザ名 例 1day-user05」を設定し、キーペアのダウンロードを押下、ダウンロードが成功したらインスタンスの作成ボタンを押下**

![ec2-12](./images/step-1/ec2-12.png "EC212")

----
**EC2インスタンスが作成開始されました。インスタンスの表示ボタンを押下**

![ec2-13](./images/step-1/ec2-13.png "EC213")

----
**作成したEC2インスタンスにて赤枠で囲った「パブリックDNS(IPv4)」の値をコピーしましょう。サーバログインのドメイン、サンプルアプリケーションであるWordpressのURLとなります。**

![ec2-14](./images/step-1/ec2-14.png "EC214")

----

## Question AMIとは
今回AMIを使用してEC2インスタンスを作成しました。このAMIについて機能、役割などについて調べてみましょう(10分)

## サーバログイン
**作成したEC2インスタンスにログインし環境の確認をしましょう。`1day-userXX.pem`は各自が作成した秘密鍵、ec2-XXXXXX.comは「パブリックDNS(IPv4)」の値です。「ec2-user@ip-10-0-0-XX」のプロンプトが返れば成功です**

```
$ chmod 600 1day-userXX.pem
$ ssh -i 1day-userXX.pem -o StrictHostKeyChecking=no ec2-user@ec2-XXXXXX.com
[ec2-user@ip-10-0-0-65 ~]$
```

**MySQLにrootユーザで接続し、所有しているデータベースの確認をしてみましょう**

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
1.このサーバに振られたIPアドレスの確認をしましょう。  
2.このサーバのデフォルトゲートウェイを確認しましょう。  
3.このサーバの基本スペック(CPU数、メモリ、DISK容量など)を確認しましょう。

## Wordpressの初期設定
**「パブリックDNS(IPv4)」の値でブラウザを開きましょう。Wordpressのサイトが開けば作成成功です。初期設定では「日本語」を選択し続けるボタンを押下**

![wordpress-1](./images/step-1/wordpress-1.png "Wordpress1")

----
**「さあ、始めましょう」を押下**

![wordpress-2](./images/step-1/wordpress-2.png "Wordpress2")

----
**データベース名、ユーザ名、パスワードを以下の設定値にしましょう。その他の値はデフォルトのままで問題ありません。設定をしたら送信ボタンを押下**

|項目|設定値|
|:-|:-|
|データベース名|wordpress|
|ユーザ名|admin|
|パスワード|wordpress|
|データベースのホスト名|localhost|
|テーブル接頭辞|wp_|

![wordpress-3](./images/step-1/wordpress-3.png "Wordpress3")

----
**インストール実行を押下**

![wordpress-4](./images/step-1/wordpress-4.png "Wordpress4")

----
**サイトのタイトル、ユーザ名、パスワード、メールアドレスを設定しましょう。パスワードは生成された値をメモなどに残しましょう。検索エンジンでの表示はチェックをしインデックスされない設定を有効にしましょう。設定したらWordPressを押下**

![wordpress-5](./images/step-1/wordpress-5.png "Wordpress5")

----
**成功したらログインを押下**

![wordpress-6](./images/step-1/wordpress-6.png "Wordpress6")

----
**ここまでの設定が間違いないか、ユーザ、パスワードを設定しログインしましょう**

![wordpress-7](./images/step-1/wordpress-7.png "Wordpress7")

----
**管理画面にログインできれば設定完了です**

![wordpress-8](./images/step-1/wordpress-8.png "Wordpress8")

----
**「パブリックDNS(IPv4)」の値でブラウザを開きましょう。Wordpressのサイトが開けば設定完了です**

![wordpress-9](./images/step-1/wordpress-9.png "Wordpress9")

----

**ここまでのオペレーションでStep1は完了です！**
