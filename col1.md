\chapter*{コラム: \\ 3次元点群取得デバイス}

ここでは，3次元点群取得デバイスの種類と特徴について紹介していきます．

3次元点群取得デバイスには，大きく分けて2つの種類があります．1つはKinect v1やXtionなどのLight Codingと呼ばれる手法を用いるもの．もうひとつはKinect v2で採用されているTime of Flight(ToF)方式を用いるものです．

Light CondingはイスラエルのPrimesense社が開発した手法で，パターン照射方式とも呼ばれます．簡単に原理を説明すると，無数のドットをパターンとして対象に照射し，そのパターンの歪み具合から距離を測定する手法です．Light Codingは究極的にはパターン照射部分と，パターンを読み取る単眼カメラさえあれば距離測定が可能なので，ローコストに実現できますが，測定精度を高めるのが難しく，また長距離の測定はできません．

一方，ToF方式は，光源から出た光が対象物で反射し，それがセンサに届くまでの光の飛行時間と光速度から対象までの距離を測定する方法です．ToFは高い測定精度を実現できる反面，これまでは非常にコストが高く，一般家庭にはなかなか普及しませんでしたが，Kinect v2は安価に実現しています．どうやっているのでしょうね？．

また，ToF方式のセンサの中で，光源にレーザー光を用いるものを特にLIDARと呼ぶこともあります．LIDARの中で近年有名なのは，Velodyne社の製品です．Googleの自動運転車のルーフでくるくる回っているのがソレです．Googleの自動運転車のルーフでくるくる回っているLIDARは，VelodyneのHDL-64という，64本のビームラインを照射し，水平全方位360°，垂直視野26.8°，最大測定距離120m， \sigma = 25[m] で \pm 2[m] の測定精度が出ます．HDL-64は性能もさることながら，価格も約900万円と言われる恐ろしいセンサです．

LIDARにももっと安価でスマートな製品が最近では開発されていて，特に最近いい感じなのが北陽電機という会社が開発しているYVT-X002という製品です．外形サイズは 70 * 106 * 95[mm] と小型ながら，水平270°，垂直視野40°，白紙に対する検出距離が25mとまあまあの性能を出してくれています．価格も40万円くらいというような話を伺っています．
ちなみに，私の研究では，北陽電機からお借りしたこのセンサをロボットに載せて使っています．