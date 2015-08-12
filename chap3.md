\chapter{3次元点群を取得するには}

ここではKinectやXtionなどのデバイスから3次元点群を取得するための手法を紹介します．
ただ，実のところ筆者はXtionもKinectも手元になくて，これらのデバイスから点群を取得したことないので，ここからの話は参考程度に受け取ってください．

# Xtion/XtionProを利用する場合	
XtionやXtion ProなどのOpenNIが対応しているデバイスから点群を取得する場合，openni2_launchパッケージ(http://wiki.ros.org/openni2_launch)を利用することができます．このパッケージはros_indigo_desktop_fullですでにインストールされているはずですが，以下のコマンドでもインストールできます．
```
# apt-get install ros-indigo-openni2-launch
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

# Kinect v1を利用する場合
KinectはOpenNI対応デバイスでありながら，ROSのopenni2_launchパッケージは対応していませんので，openni_launchパッケージを利用します．こちらもros_indigo_desktop_fullですでにインストールされているはずですが，以下のコマンドでもインストールできます．
```
# apt-get install ros-indigo-openni-launch
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

# Kinect v2を利用する場合
Kinect v2は，libfreenect2を利用したROS用インタフェースパッケージ(https://github.com/code-iai/iai_kinect2)を開発している方がいらっしゃるので，そちらを利用するのがよいのではないでしょうか．導入手法などはそちらに詳しいので，そちらを参照してください．