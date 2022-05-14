# sumo user conference 2022
<!-- * [HP](https://www.eclipse.org/sumo/conference/) -->
* 2022/5/9-11, online, 視聴者～100名程度
* SUMO開発チームはドイツ航空宇宙センターのTransportation Systems部門（約250名）
    * Research Topicは，安全・環境思考の輸送システム，スマートシティのためのデジタルツイン，自動運転など
    * スピンアウトしたベンチャーが[bashboard形式でsimを実行できる環境](https://sesam.co4e.com/)を提供中．トライアルで10 [simulation/day] 実行可能．
* 開発，交通工学の人々が多い印象．例えば，発表への賞を決める投票で，信号機制御に強化学習を入れた話しが人気だった．
* co-simulation，車以外の交通流，信号機，クラウド，3D化などが最近のキーワードの模様
* 個人的に有用だったのは，co-simのMOSAIC，交通量データダッシュボードのtelraam，

---

## SIM環境，ツール
### [MOSAIC:A Multi-Domain and Multi-Scale Simulation Framework for Connected and Automated Mobility](https://www.eclipse.org/mosaic/)
複数のシミュレータやアプリケーションを連携させるco-simulater．
交通流をSUMO，その他，ネットワークレイヤーや独自のアプリケーションを連携させられそう．

### [KI4RoboFleet](https://keim-hs-esslingen.github.io/ki4robofleet/)
POIを行き来する自動運転車をSUMO上に走らせるためのツール．
POIの位置，パーキングの追加，POI間を行き来するデマンドの設定，
シミュレーションの可視化，走行台数で経済や環境に与える影響を定量的に評価できる．
[分かりやすいデモ](https://www.youtube.com/watch?v=X5AYifgP65g)

### [TAPAS-SUMO](https://github.com/DLR-VF/TAPAS)
ドイツのtrip調査などを用いて，簡単かつ尤もらしいデマンドを生成するツール．[論文](https://elib.dlr.de/113300/1/Heinrichs_et_al_2017_Introduction%20of%20car%20sharing%20into%20existing%20car%20fleets%20in%20microscopic%20travel%20demand%20modelling%20-%20Preprint.pdf)
家庭の規模・年収・車保有台数・住所，年齢や性別，免許所持当を考慮し，家庭パタン→人・車パタン→人のアクティビティを生成．アクティビティと施設までの距離等を考慮して行き先生成．移動方法はロジットモデル（パラメタはSrV2008/MiD2017から生成した模様）．背景交通やネットワークは外部で生成，TAPASで生成したデマンドをSUMOにいれ，調整？

### [veins](http://veins.car2x.org/)
車車間通信レイヤを扱うシミュレータ．SUMOと連携できる．

### [Traffic-Interventions](https://github.com/WSL-IIITB/Traffic-Interventions)
ネットワークファイル向けのツール．
lane priority，#lane，allowed vehicle typesなどを操作可能．

---

## データ
### [telraam](https://telraam.net/#6/60.081/13.173)
交通量のダッシュボード．主に欧州にセンサが設置されていて，日ごと，時間帯ごと，交通種別ごとの交通量を知ることができる．例えば[リンク](https://telraam.net/nl/location/9000002980)
ラズパイとカメラで構成されたデバイス（2022.5.14時点で約1.7万円）を購入し，プラットフォームに接続することで[参加できる](https://telraam.net/en/self-measure)．
[個人参入か，団体としての登録も募集している](https://telraam.net/en/what-is-telraam)．

[Linkdin](https://be.linkedin.com/company/rear-window-bv?challengeId=AQHRySoMHjqdrwAAAYDAoUi4nyMe2EssF97Ebyrg2onJaWoTaKniYg_9mmQMbQc827u3d4QORqrctG9b3beklPjKO2AnFtoVQg&submissionId=17571cec-4cdb-ee16-8a70-b26ac84f7e1b)によれば，2019年から，Transport & Mobility Leuven（ベルギーの交通コンサル），Mobiel 21（NGO），Waanz.in（digital studio，fablabみたいなもの？実態不明）でプロジェクトスタート．2021にスピンアウトしたとのこと．ベルギーと欧州から資金を引っ張ってきた？
> Telraam was developed with the support from the Smart Mobility Belgium fund by the Belgian federal government and European Union’s Horizon 2020 research and innovation programme in the WeCount-project, under grant agreement No. 872743.

### [viscando](https://viscando.com/applications/traffic/)
交通量データを持ってる？

### [streetlightdata](https://www.streetlightdata.com/)
OD等のデータを販売．交通量をプラットフォーム上で可視化できる．
[demo](https://www.youtube.com/watch?v=gpGRIeOI5j4&t=1s)

### [地形データ：Shuttle Radar Topography Mission](https://www2.jpl.nasa.gov/srtm/)
[日本語の解説サイト](https://www.kashmir3d.com/srtm/)あり．

---

## 発表メモ
### High fidelity modelling of traffic light control with xml logic representation
SUMOの信号機制御は，XMLでサイクル時間等を実装するか，TraCIによる外部プログラム（Python, C++, Matlab, Java）からの制御だった．
この発表では，XMLの拡張で，if-thenやそのチェインルールを実装可能にした．

### Simulating Digital Rail: From PlanPro railway plannings to SUMO simulations
鉄道のデジタル化を進めるうえで，新しいデバイスを既存のシステムに入れられるか保証したい．
　既存のプランニングツールからSUMOに変換するコンバータを作成．
https://github.com/arneboockmeyer/planpro-sumo-converter
https://github.com/arneboockmeyer/planpro-generator

###  A dynamic model for ride-matching problem in multi-hop ride-sharing system
ライドシェアにおけるマッチングの難しさは，マッチングするドライバ数と乗客者数が単一か複数かによって異なる．
複数のドライバと複数の乗客を扱うとき，一台の車にODの異なる乗客がのる．
そのため，①乗客のtripをsub-tripに分割，②ドライバのsub-tripを繰返し計画する必要がある．

### Multi-Modal Traffic Simulation Calibration and Integration with Real-Time Hardware in Loop Simulator
Hardware-in-the-loop（車のECUをテストする仮想環境）をTrafficに拡張．
dSpaceというHardware-in-the-loopのアプリケーションとSUMOを連携させた．
SUMOはGAを使って，再現する区画への流入量をキャリブレーション．

### Simulation of surrounding traffic in a driving simulator – Coupling Sumo, RoadRunner and Unity
RoadRunnerでつくった環境モデルから，SUMOとUnityを実行．
二つを連携させて，Unityは3D表示，SUMOは交通流計算を担う．

### Implementation of a Perception Module for Smart Mobility Applications in Eclipse MOSAIC
物体認識を実装することができる，co-simを構築．

### A Co-Simulation Middleware: Virtual Testing of Automotive Applications with Multiple Simulators
CARLAとSUMOを連携，OpenDRIVEに準拠．現在，非公開？

### Building a real-world traffic micro-simulation scenario from scratch with SUMO
SUMOを使ったシミュレーション構築手順をコードレベルで紹介．

---
## その他

### calibration指標
発表中に見かけたものをメモ．

#### Point Mean Relative Error (PMRE)
$\sqrt{\sum_{i} \frac{T_{obs} - T_{sim}}{T_{obs}}}$
$i$ : section
$T$ : 平均旅行時間

#### Mean Absolutely Percentage Error (MAPE)
$\frac{1}{N} \sum_{i=1}^N \frac{|\hat{y}_i - y_i|}{y_i}$
$\hat{y}_i$ : シミュレーションの旅行時間
$y_i$ : 正しい旅行時間

### 用語等
* [RoadRunner](https://jp.mathworks.com/products/roadrunner.html) : MathWorks社製の自動運転向け3Dシミュレータ．道路標識やガードレールを設置することが可能．
* OpenDRIVE：[ASAM(Association for Standardisation of Automation and Measuring Systems)](https://ja.wikipedia.org/wiki/Association_for_Standardisation_of_Automation_and_Measuring_Systems)が標準化する道路ファイルフォーマット．[道路形状や車線，交差点だけでなく，標識・信号機や道路の素材・凹凸も扱えるらしい](https://www.mpcnet.co.jp/topics/2019/2020_10_01_ASAM_OpenDRIVE.pdf)．設立は1998年，，日本の自動車業界も2010年代前半に参入しはじめた模様．OpenDRIVE自体は，アメリカの開発会社から移管した[痕跡あり](https://www.mscsoftware.com/ja/news/vires/announces/transfer/opendrive/standard/toasam)．


