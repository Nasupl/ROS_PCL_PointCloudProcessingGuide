\chapter{開発環境の構築}
本書では，開発にUbuntu14.04を使用する前提で解説を進めていきます．

# ROS本体の導入
ROSはIndigoを導入します．
ROS本体の導入については，ROS WikiのROS Indigo の Ubuntu へのインストール( http://wiki.ros.org/ja/indigo/Installation/Ubuntu )を参照してください．
ROSのデフォルト構成は，すべてのデスクトップ環境(ros-indigo-desktop-full)としてください．

# PCLの導入
ROSでPCLを利用する場合，必要となるものは，すべてのデスクトップ環境(ros-indigo-desktop-full)を導入した時点で導入されますが，念の為以下のコマンドで必要なものをインストールしましょう．
```
# apt-get install ros-indigo-perception-pcl libpcl-1.7-all-dev
```

# ワークスペースとビルド設定ファイルの用意
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
ここではワークスペースの名前を *catkin_ws* としましたが，別の名前でも構いません． *catkin_ws* 以外の名前をつけた方はこれ以降その名前と読み替えつつ読み進めてください．

次に，catkinでビルドするパッケージの準備をします．次のコマンドを入力してください．
```
$ cd ~/catkin_ws/src
$ catkin_create_pkg ros_pcl_tutorial std_msgs sensor_msgs 
roscpp pcl_ros pcl_msgs pcl_conversions libpcl-all-dev
```
これで~/catkin_ws/src直下にros_pcl_tutorialディレクトリが作成され，ディレクトリ内に以下の2つのファイルが生成されたのではないでしょうか．

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

**Invoking "make cmake_check_build_system" failed** などのエラーメッセージが赤文字で表示されなければビルド成功です．
