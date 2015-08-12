\chapter{はじめに}

# はじめに
2010年にKinectが発売されて以来，それまで数十万円〜数千万円のセンサでしか取得できないものであった3次元点群が，誰でも簡単に取得できるようになりました．一方で3次元点群の処理は各種フィルタリング，特徴量の推定，モデルフィッティングなど多岐に渡り，それぞれの処理を一からコーディングしていくのは，非常に心の折れる作業となります．Point Cloud Library(PCL)は，そのような各種点群データの処理のためにオープンソースで提供されているライブラリとなっています．

またロボット分野では，2007年より始動したオープンソースのロボット向けフレームワークであるROSが非常に注目を受けています．ROSは，ロボットに用いる通信，ビジュアライズや座標変換などの機能/ツール群，またパッケージシステムを提供するフレームワークであり，5月に行われたDARPA Robotics Challengeでも出場23チーム中18チームで使用されているなど，世界で最も普及しているロボット向けフレームワークと言っても過言ではないでしょう．

本書では，点群処理をロボットの制御に組み込みたいと考えている読者を対象に，ROSとPCLを併用していく際の環境構築 ~~，開発に際しての基本事項を中心に，ROSとPCLを用いた点群処理の入門部分~~について解説していきます．

また，本書は未完成であり，今後，点群処理の入門部分をはじめとした大幅な加筆修正を行う予定です．そのため，本書はGtihubで公開し，加筆や修正を行うたびにそちらに反映させていく予定です．また，訂正が必要な部分については皆様からのPull requestなどもどしどしお待ちしておりますので，ぜひともよろしくお願いします．


## PCLについて
PCLは先述したように，オープンソースの点群処理ライブラリとなっています．PCLについては詳細な説明を省きますので，詳しくはpointcloud.orgを参照してください．

## ROSについて
ROSは先述したようなロボット向けのオープンソースフレームワークです．通信，ビジュアライズや座標変換などの機能/ツール群，パッケージシステムを提供してくれるロボットのソフトウェア開発にはもはや必須とも言えるツールとなっています．
ここでは詳細な説明を省きますので，詳しくはROS.orgなどを参照してください．