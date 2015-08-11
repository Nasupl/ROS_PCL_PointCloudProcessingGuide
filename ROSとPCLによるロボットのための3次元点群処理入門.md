<!-- ROSとPCLによるロボットのための点群データ処理入門 -->
# はじめに
2010年にKinectが発売されて以来，それまで数十万円〜数千万円のセンサでしか取得できないものであった3次元点群が，誰でも簡単に取得できるようになりました．一方で3次元点群の処理は各種フィルタリング，特徴量の推定，モデルフィッティングなど多岐に渡り，それぞれの処理を一からコーディングしていくのは，非常に心の折れる作業となります．Point Cloud Library(PCL)は，そのような各種点群データの処理のためにオープンソースで提供されているライブラリとなっています．

またロボット分野では，2007年より始動したオープンソースのロボット向けフレームワークであるROSが非常に注目を受けています．ROSは，ロボットに用いる通信，ビジュアライズや座標変換などの機能/ツール群，またパッケージシステムを提供するフレームワークであり，5月に行われたDARPA Robotics Challengeでも出場23チーム中18チームで使用されているなど，世界で最も普及しているロボット向けフレームワークと言っても過言ではないでしょう．

本書では，点群処理をロボットの制御に組み込みたいと考えている読者を対象に，ROSとPCLを併用していく際の環境構築，また開発に際しての基本事項を中心に，ROSとPCLを用いた点群処理の入門部分について解説していきます．

また，本書は未完成であり，今後大幅な加筆修正を行う予定です．そのため，本書はGtihubで公開し，加筆や修正を行うたびにそちらに反映させていく予定です．また，訂正が必要な部分については皆様からのPull requestなどもどしどしお待ちしておりますので，ぜひともよろしくお願いします．


## PCLについて
PCLは先述したように，オープンソースの点群処理ライブラリとなっています．PCLについては詳細な説明を省きますので，詳しくはpointcloud.orgを参照してください．

PCL解説記事の紹介

## ROSについて
ROSは先述したようなロボット向けのオープンソースフレームワークです．通信，ビジュアライズや座標変換などの機能/ツール群，パッケージシステムを提供してくれるロボットのソフトウェア開発にはもはや必須とも言えるツールとなっています．
ここでは詳細な説明を省きますので，詳しくはROS.orgなどを参照してください．

ROS解説記事の紹介

# 開発環境の構築
本書では，開発にUbuntu14.04を使用する前提で解説を進めていきます．
## ROS本体の導入
ROSはIndigoを導入します．
ROS本体の導入については，ROS Wikiの"ROS Indigo の Ubuntu へのインストール( http://wiki.ros.org/ja/indigo/Installation/Ubuntu )"を参照してください．
ROSのデフォルト構成は，すべてのデスクトップ環境(ros-indigo-desktop-full)としてください．

## PCLの導入
ROSでPCLを利用する場合，必要となるものは，すべてのデスクトップ環境(ros-indigo-desktop-full)を導入した時点で導入されますが，念の為以下のコマンドで必要なものをインストールしましょう．
```
\# apt-get install ros-indigo-perception-pcl libpcl-1.7-all-dev
```

## ワークスペースとビルド設定ファイルの用意
ここでは，ROS標準のビルドシステムであるcatkinを利用するためのワークスペースの設定と，ビルド設定ファイルの準備をします．
まずは，以下のコマンドでワークスペースを作成してください．
```
$ mkdir -p ~/catkin_ws/src
$ cd ~/catkin_ws/src
$ catkin_init_workspace
$ cd ~/catkin_ws
$ catkin_make
```
これでワークスペースの準備が出来ました．
ここではワークスペースの名前を * catkin\_ws * としましたが，別の名前でも構いません． * catkin\_ws * 以外の名前をつけた方はこれ以降その名前と読み替えつつ読み進めてください．

次に，catkinでビルドするパッケージの準備をします．次のコマンドを入力してください．
```
$ cd ~/catkin\_ws/src
$ catkin\_create\_pkg ros\_pcl\_tutorial std\_msgs sensor\_msgs roscpp pcl\_ros pcl\_msgs pcl\_conversions libpcl-all-dev
```
これで~/catkin\_ws/src直下にros\_pcl\_tutorialディレクトリが作成され，ディレクトリ内に以下の2つのファイルが生成されたのではないでしょうか．
* CMakeLists.txt
* package.xml

しかしながら，このままではビルドが通らないのでCMakeLists.txtを一部以下のように変更します．

* 変更前
```
find_package(catkin REQUIRED COMPONENTS
  lib-pcl-dev-all
  pcl_conversions
  pcl_msgs
  pcl_ros
  roscpp
  sensor_msgs
  std_msgs
)
```

* 変更後
```
find_package(catkin REQUIRED COMPONENTS
  pcl_conversions
  pcl_msgs
  pcl_ros
  roscpp
  sensor_msgs
  std_msgs
)
```

変更を加えたら次のコマンドでビルドを実行してみましょう
```
$ cd ~/catkin_ws
$ catkin_make
```
* Invoking "make cmake\_check\_build_system" failed *などのエラーメッセージが赤文字で表示されなければビルド成功です．

# 3次元点群を取得するには
ここではKinectやXtionなどのデバイスから3次元点群を取得するための手法を紹介します．
ただ，実のところ筆者はXtionもKinectも手元になくて，これらのデバイスから点群を取得したことないので，ここからの話は参考程度に受け取ってください．

## Xtion/XtionProを利用する場合
XtionやXtion ProなどのOpenNIが対応しているデバイスから点群を取得する場合，openni2\_launchパッケージ(http://wiki.ros.org/openni2_launch)を利用することができます．このパッケージはros\_indigo\_desktop\_fullですでにインストールされているはずですが，以下のコマンドでもインストールできます．
```
\# apt-get install ros-indigo-openni2-launch
```
Xtionを接続したら，以下のコマンドをそれぞれ別の端末で実行してください．

* 端末1
```
$ roscore
```

* 端末2
```
$ roslaunch openni2_launch openni2.launch
```

正常に動いていれば，camera/depth/pointsトピックで点群が配信されているのではないでしょうか．

## Kinect v1を利用する場合
KinectはOpenNI対応デバイスでありながら，ROSのopenni2\_launchパッケージは対応していませんので，openni\_launchパッケージを利用します．こちらもros\_indigo\_desktop\_fullですでにインストールされているはずですが，以下のコマンドでもインストールできます．
```
\# apt-get install ros-indigo-openni-launch
```
Kinectを接続したら，以下のコマンドをそれぞれ別の端末で実行してください．
* 端末1
```
$ roscore
```

* 端末2
```
$ roslaunch openni_launch openni.launch
```
正常に動いていれば，camera/depth/pointsトピックで点群が配信されているのではないでしょうか．

## Kinect v2を利用する場合
Kinect v2は，libfreenect2を利用したROS用インタフェースパッケージを開発している方がいらっしゃるので，そちらを利用するのがよいのではないでしょうか．導入手法などはそちらに詳しいので，そちらを参照してください．

---ここascmac.styのitembox使う
\begin{itembox}[l]{筆者はどうやって3次元点群を取得しているのか？}
筆者は研究で3次元点群を利用していますが，先述の通りKinectやXtionなどのデバイスは利用していません．筆者の研究は，屋外で，また処理したい対象までの距離が5mを超えるような(Kinect v1は最大測定可能距離4000[mm])環境で取得した点群を扱う必要があるため，KinectやXtionなどの所謂Light Codingと呼ばれている方式のデバイスは利用できないのです．
そこで利用しているのが，Kinectでもv2から採用されるようになったTime of Flight方式の装置です．Time of Flight法は，光源から出た光が対象物で反射し，それがセンサに届くまでのヒカリの飛行時間と光速度から対象までの距離を測定する方法です．特に，レーザー光でそれを行うものをLIDARと呼びます．
LIDARの中で有名なのはVelodyne社の製品です．Googleの自動運転車のルーフでくるくる回っているのがソレです．Googleの自動運転車のルーフでくるくる回っているLIDARは，VelodyneのHDL-64という，64本のビームラインを照射し，水平全方位360°，垂直視野26.8°，最大測定距離120m，1\sigma=25mで\pm 2mの測定精度が出ます．HDL-64は性能もさることながら，価格も約900万円と言われる恐ろしいセンサーです．
私が所属している研究室ではそのような恐ろしいセンサを購入できる予算もありませんので，2次元LIDARを3次元的に動かすなど，工夫して利用することによって，3次元点群を取得しています．また，今年度からは北陽電機が開発中のYVT-X002という3次元のLIDARを貸して頂いて，私の研究にはそちらを利用させていただいたいます．
\end{itembox}


# 基本の点群処理
## ROS-PCL間の基本的事項
引っかかりやすい部分について解説．fromROSMsgとか．

## VoxelGrid
ソースコードと解説

## Outlier Removal
ソースコードと解説

## SAC
ソースコードと解説

## Euclidean Cluster
ソースコードと解説

# おまけ
## PCL関連ツール
pcd2hogeなどなど
## ros関連ツール
pcd_publisherみたいなやつ