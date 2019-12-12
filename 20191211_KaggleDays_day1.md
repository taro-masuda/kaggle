## Intro
- Anna Motoya
    - 12カ国以上のトップDSが集結
    - 8 GM, 37 Masters, 65 experts
    - 共に学び、教えあい、コネクションを作る
    - LOGICAI: KagglerがKaggle Daysの初期を立ち上げた 
    - Kimihiko Kitase: Head of enterprise Marketing, google cloud
        - Host
        - attended DS dojo event in Washington
---
## Ben hammer: leveling-up competitions
- kaggle CTO
- global GDP per capita: ここ５０年で指数的に増えている
- 平坦になっていくかそのまま進むかはAIテクノロジー次第
    - driven by us! we have opportunity.
    - launched a decade ago.
        - MNIST: supervised learning
        - SVM, naive bayes, LR など
    - **Prediction** -> code -> simulation
        - 364 competitions, 400,000 people submitted
        - consistent hold-out criteria
        - 2011: random forests worked well
        - 2012: GBM achieved slighly better
        - 2013: neural nets (affected by Hinton)
        - Present: DL+GBM

1. Tough to collaborate -> Kaggle Notebooks
2. Tough to get access to high-quality data -> Kaggle Datasets through docker containers
        - easy to publish datasets!
        - 20,000 データセットがある
        - Your TaskをSuggestしてくれる項目もある

## Prediction -> **code** -> simulation
- BERT is now available
- 文字から写真検索、文字オンライン翻訳、眼底病気の検出（Kernel onlyの話？）
- we care about latency, memory, datasets, explainability,
- 16 code competitions, 32000 submitted
- Quoraの例：1st place solutions: Much simpler models than ensembled
- Freesound: with narrow range of GPU
- create dog image competition: narrow computer resource within 9 hours

## Prediction -> code -> **simulation**
- very much like toys, games
- Connect X Beta competitions: include other player is the 
    - ロボットアームで操作するコンペ
    - ジャワの農業の50%人が足りないものを0人に近くする解決につなげたい
- Kaggle Learn!
- AI Dungueaon: Open AI: Actions in virtual world
- AI interacting environments
- 次の変曲点が楽しみ
---
## Onoderaさん (GM) Essential techniques for tabular competition
- SantaはMLではない
- DeNAのランク制度の説明：1回会社辞めればランクSになれる
### see the data
- Some statistics: 最初に統計量をとる
    - #NULL, #Uniquesで使えるデータかどうかが分かる
    - 連続値以外のものにも使えるがTOP10の値を見たりもする
- Feature vs target
    - Trainの中でターゲットがどう分布しているかを見る
    - barplot with target mean
    - Venn diagram: trainとtestでどれくらい同じカテゴリがあるかを見る
        - 全く共通してない場合は後で考えないといけない
    - 我流
        - Subset by id（インスタカート）
        - reorderを当てるコンペ
        - order_number＝時系列
        - reorder が頻繁に買っている（ソーダ、ジャーキーなど）
        - たまにしか買わないものを当てるのが肝
        - コーラを8回目だけなぜか買ってない
            - Fridge Pack Cola: コーラの代わりに買っていた
            - English muffinが代替品かもしれない、どうやって図るか
            - 連続する0,1または1,0の特徴量を作った
                - 全ユーザ全オーダーに対してやっている
                - ココナッツミルク->Soy beansなど33人が代替品として買っている
                - Garbanzo beans -> Tofu
                    - 代替品っぽくないじゃないですか？
                    - 注目はCo-occurrence（同時に買った回数）= 1.9 （平均で）　代替品として成立してない
                    - garbanzo beans -> organic garbanzo beansは代替品だろう（Co-occurrence = 0.02）,逆もまたしかり
                - Fat free Milk -> organic Skim milk
                    - 1.8 (co-occurence=0.08)
                - カンパリトマト->スイートカンパリトマト
                - Bag-of-organic bananas -> banana
                    - 一番バナナが売れている
                    - 全員バナナ買うというベンチマークもまあまあうまくいく
                    - co-occurence=0.01
            - KDD-cup(MOOCのオンライン学習の辞めちゃう人予測)
                - Object列：何のチャプターを履修したかが人によって違う
                - 最初3つはマスト、4つ目以降は自由に履修するパターン
                - Objectが何番目に見られるかを見た->ばらつきを散布図でプロット
                - plot(kind=scatter, order-mean, order-std)
        - Sorting: 単純だが強い分析手法
            - Home Creditの例：銀行の与信を行う
            - 年金の合計でソートしてみると、同じValueかつその他の項目も全て同じというIDがいくつかあることに気づく
            - SK_ID_CURRが違うので、違う人として扱ってしまいがちだが、同じ申し込み
            - 日付の引き算で特徴量を作ると、違うIDなのに一致することがある
            - 全く同じ人、かつ別の時系列の人であることが判明した
            - lag feature(過去や未来からの差分), lead feature,rate of change, aggregationなどで良いモデルができる
        
- Kaggle Expert RTA
    - 2 bronze medal 2個必要（TOP 40%結構難しいんですよー）
    - 制約：without ML
    - IEEE: 0.93818 がボーダー
        - notebookのベストスコア-> 1個目ゲット
    - Google Analytics Customer Revenue Prediction
        - Sample Submissionで銀メダル獲得
    - 8ヶ月か来る

- Good コンペランキング
    - 制約：No leak
    - Home CreditやIEEEはユーザIDを特定する話なのでナンセンス（当てに行く意味がない）
    - 制約2: Meaningful validation and PublicLB
    - 制約3: 楽しいということ
    - 3位：Otto　シンプルFeature, Representiatuve multi-class, practice for ensemble
    - 2位：Prasticc: Social good, clean data, very few train data（マルチクラス）見えない星をどう穴埋めするかがポイント
    - 1位：Instacart　Clean data, REcommendation focusng on reorder(ホットなユーザだけ当てに行けばOK), metric(F1-value)の把握(後処理)が必要、Augmentationが結構簡単にできる（最後から1個前、2個前などを学習に使える）
---
## タリンさん
- host team 4人だけ
    - Tarin (Unranked)
    - Mikel (Cambridge univ B1, anokas, GM)
    - Alex (学生)
    - Asanobu (CODH/NII、上司)
    - 日本から3回目の主催
- 背景
    - 古文書がたくさんあるのに0.01%の人しか読むことができない
    - イラストは認識しないで文字だけを認識したいというタスク
    - KuroNETというベースラインを超えてほしい（Private 12位だった）
    - TOP5に同じ金額：ほぼ運だから（3000ドル）
- ひらがなと漢字の頻度
    - 偏っている、とても
- Train data
    - 画像、CSV（ユニコードとBounding box）
- Test data（4500枚）
    - 画像のみから文字を読む
    - 2500枚はダミー（採点されない）->数が少ないのと、Cheating対策のため）
        - どの本からきたか分からないように匿名化するため
- Why Playground?
    - 元々国の文書なので、他の研究にも使えるものにしなきゃいけない->手法を共有しないといけない。Leakageを防ぐことができないのでPlaygroundになった
- Competition design
    - 分かりやすいタスク、Metric
    - 日本語だけど誰でも参加できるように設計する（日本語が有利にならないように）-> Data Preparation はこっちでやって、モデリングに集中できるようにした
    - Eval: **modified** f1 score. 中心がBounding Boxに入っていれば正解とする
        - IOUじゃなくてあくまで文字の認識を大事にしてほしいから使わなかった。文字と文字が重なりまくっているので文字の分類に集中してくれないと困るので
- result
    - 日本人は3位
    - 反省点は1位と2位が同じ点数（小数点以下3桁まで）
        - 珍しい文字がちゃんと読めたかを見たいのでF1 scoreはまだよくなかった
- Winning solutions
    - Object detection algs worked well
        - End-to-end
        - Character Detection -> Character Classification
            - Detectionはうまくいきやすいが、Classificationは改善の余地あり
        - YOLOだけうまくいかない（クラス数が少ないのに対して、漢字やひらがなは何千クラスとあるので）
    - Mixup + Ransom Erasing
        - 薄い文字を重畳する
        - 文字の一部を消す
    - Augmantations library(albumentation)
- What we learned
    - pseudo labeling: 本が違うので文字が違う（TrainとTestで）
    - ImageNot or COCO pretrain modelsもうまくいった
    - multiscale train and test (バラバラのサイズでうまくいく)
- うまくいかないこと
    - 言語モデル（専門知識が必要）
    - Creating more character(クジラ認識など)：文字はほぼ同じ向きなので
    - 漢字かな交じり と 漢文 で違う
        - 前者は漢字は単なる音として使っているのでうまくいかない
- 注釈が認識できない
- test dataがたりない：60万文字しかない
    - Sequenceが分かれていない。目で見てもどこが区切りか分からないので言語モデルが作れない
- 物理の本（漢字とかたかな）、物語（漢字かな）、歴史（漢字だけ）
    - 言語モデルを作るのが大変すぎる
### Future
- Test dataの文字クラスが少なく簡単すぎるタスクだった、頻度高い1000文字だけやってれば0.95になる
- モデルの問題よりもデータセットを増やして学習していくのが今後の課題
- 江戸時代の本が読めても、平安時代の本が読めない（見た目が違うので）
- 画像とテキストボックスはほぼ印刷されたもので手書きのものは入ってない
- 終わりではなくスタート
- お金があっても買えないものはあるが、皆さんの本気を買うことができる
---
## Tomohiro Takesako @tikutiku
- 東大PhD
- 2018からKaggle, Master
- 長野県在住
### Dive into NN competitions ビギナー向け
- 最近NNコンペ多い、（Image Sound and NLP）
- select -> baseline -> improve
- 計算資源が必要で大変なことも
- データが自分にとって興味があって、中身を詳しく見たいかどうかが大事

- 鉄コンペもSegmentationで、どっちやるか迷った、雲画像の方が見て飽きなかった
- ベースラインにしっかり時間を録ることが大事
    - EDA, Validation stra, Preprocessing, Augmentations,
    - model, loss functions, optimizer, scheduler,
    - postprocessing


### 知りたかったこと
- 塩コンペ：
    - CVもLBも上がらず苦労した
    - Discussionを再試してもうまくいかなかった
    - 見当はずれのチューニングをしていた
- 今作ったモデルを長時間回すことに意味があるのか
    - Augmentationどうするか
    - Discussionの内容再現できないのはなぜか
    - スコアの伸ばし方
- やってること
    - Loss history, metric history を見て変なところw見る
    - CVが上がったのにPublicLBが下がってないか見る
    - 例）Trainは下がってるがValidが下がっているのか微妙な動き
    - Metrics, lrやWeight decayを2パターンずつ試した
        - Loss functionやAugmentationをいろいろ試行錯誤
        - 20エポックから40エポックに増やした
    - Augのときはどんな画像が作られたか目で見たほうがいい
        - 回転やシフトの時、ボーダーがどうなるか？（反転うまくいかなかった、黒く塗りつぶしたら上手くいった）
        - ランダムスケーリング上手くいかなかった（原因は定かじゃない、細かいセットアップにもよりそう）
- 他の人のが再現できない理由
    - ディテール、計算リソースが違うことに起因する
        - preprocessing
        - training process
        - hyperparameters
        - implementation details
    - discussionは参考程度に

- 改善
    - validationの確立（ランダムシードの固定、TrainやTestの分布の違いを見るなど）
    - 前処理,preprocessing, model変える、
    - レイヤーを変えるのはあまり効かなそう
    - スタンダードな解法を知る
        - 論文を読む
            - NNについて自分で隅から隅まで実験しない。論文により効率的に実験結果をもらう
        - discussionを読む
        - kernelを読む
        - 他コンペのソリューションを読む
        

### Case study
#### 雲コンペ
- セグメンテーションのタスク、ラベルがノイジー、シェイクに気を付けないといけない
- アノテータが主観でラベルを付けてしまっていた
- ZooniverseというHPで主観的につけられている（アバウト）
- BCE：ローカルなロス -> LovaszHingeロスでちょっとうまくいった
- BCE + LovaszHinge + more Aug.で精度が上がった
#### GANコンペ 
- 初めてのGANコンペ
- MiFIDというメトリック（画像の品質と多様性を測る指標）
- Kernel only 9h, no external data, スクラッチでGANを書かないといけない
- 120の犬の品種がある
- trainが高品質とは限らない
- アスペクト比が7.7ゴミがあったりする（犬の端切れ）->データをよく見よう
- モデルを、簡単なDCGANから複雑なものに変えていくとうまくいったが、最後にBigGANでlearning rateを変えるとShake downしてしまった
- BigGANが重要だが、ディテールが大事（みんな違う設定でBigGANを使っていた）
    - オリジナルを小さくしたものをみんな独自に作っていた

### まとめ
- まずは初心者はBaselineに時間を割く
- Practice, practice, and practice
- details are important
---
### Yuji Hiramatsu , master@UT物理
- AXA Japan, Senior DS
- クマの写真

### 自己紹介
- 本
- UTのSecond teacher
### motivation
- MLの前からいろんな手法があった
- 多重代入法というやり方がある e.g. TitanicではRのMICEというパケージ
- まず「欠損値のまま取り扱う」：NanをそのままXGBoostに放り込む
### 統計的見地からの欠損値の話
- 欠損値の発生の仕方は三種類ある
    - MCAR
        - x1: 欠損がランダムな場合
    - MAR
        - x2にのみ依存して欠損している
    - NMAR
        - x2とx1両方に依存して欠損している
- LD, PD; 欠損を削除する(非推奨)
    - リストワイズ除去
        - 欠損値が1つでもあると全部除去する
    - ペアワイズ除去
        - x1-x3を使うとき、x1だけかけていれば残す、
- 単一代入法（値を補完する）
    - Titanicカーネル、平均値や中央値で代入
    - 弱点：例えばMARの場合、欠けているところをすべて平均にすると罰点が変なことになっちゃう->回帰式をつくってあげないといけない
    - いろいろアルゴリズムがあるが掘り下げない
    - Underestimate the variance：予測値の**分散**が過小評価してしまう
- 多重代入法
    - M個の代入したデータセットを作る
    - モデルを作ってM個分の平均値で予測する
    - 効果：不確実性はUnderestimateしない
    - Fancyimpute, impyute, IterativeImputer Norm, MICE, Amelia
        - 精度だけを重視するKaggleで重要かどうかはわからない
- MICEのアルゴリズム
    - 既存の値をランダムに放り込む->欠損してないものを考慮して欠損値を予測するモデルを作る->値を埋め込めたので今度はx3を予測する->繰り返し
- Missing value treatment in XGB
    - review: 決定木をシーケンシャルにつなげて予測値を作る（残差を次の期で予測）
    - missing value flow
        - NANがない状態で最適な閾値を求める->NANをどっちに割り振る（Default direction）か決める: 右と左でどっちがLossが少なくなるかで決める＝NAであることとターゲットの値を関連付けていることになる
### ナイーブ実験
- BNP Paribas cardifのクレームデータを使用；欠損値がいっぱいあったので
- 131種類、11万行
- 僅差の戦い：Logloss小数点以下第4位の戦い
- Baseline model vs baseline + MICE
    - baseline: いらないカラムを録って21列にする->ordinal encoder
    - 主観操作を入れないようにHyperOptでチューニングする(seed 2019を使う)
    - Trainingには2020-2024のシードを使って5つ分学習
    - 50%くらいは欠けてない列だが、めっちゃ欠けているものもある
    - 残念ながらランダムな欠け方ではない
- MICE: 計算コストが高いので簡単なものを使っている
    - M=30(疑似データセットを30個使って平均を取る)
    - max-iteration = 学習繰り返し最大20回
- 結果
    - OOF: MICEをしたほうが良さそう感はある; M=5から安定して精度出やすい
    - Public Score: 全然ダメ
    - Private: NAをそのままXGBに突っ込む方がましな結果になっている
- 考察
    - XGBはNAであることを活かしている（特徴としている、Targetと関連付けている）から強い
    - RoundKとRoundLで2回同じ特徴を使うとき割り振られる値が変わってくる->多重代入法の昨日が備わっている
    
- じゃあMICEは？
    - NAである特徴を追加してやってみた、公平性のためにXGBも同様にした
    - 結果：NAフラグを付けたほうがOOF, public, Privateすべてで精度が良い
    - 計算コストに余力があれば良いスコアを出す可能性がある
---
## Ryuji Sakata @Kyoto Univ. from Panasonic
- Encode categorical features for GBDT
### 1. エンコーディングテクニックof カテゴリ特徴量
- 代表的なのはOne-hot encoding：0,1のフラグを付ける
- 水準の数が多い（high-cardinarily）だと一気に列名が増える
- Label Encoding; GBDT系専用：辞書順に並び変えて数字に変換する
- Target encoding: ターゲットの平均値に置き換える
    - プロダクトIDという各水準を小さいものから並び替えているところが肝；しかしリークのリスクがある。例えば値が1個しかない場合は明らかなリーク
    - out-of-fold: 
        - Trainfold部分で平均を計算して、自分自身の平均は使わないのでLeakageを防げる：推論時は全サンプル使ったりする
        - Foldの数を大きくしすぎると過学習する
    - LightGBM: カテゴリカル特徴量サポート
        - モデリングの前にやるのではなく、分割したSplitごとに順番を並べるTarget encodingをしている
        - 木が増えていくごとにTarget encodingの値がDynamicに変わっていく
        - 当然、リークの危険性は存在するはず min-data_pergruopやCatbosstで抑えれるという対策がある
- その他
    - Feature hashing
    - Freq encoding: 同じ数出現しているものがあるのでJackさんはあまりやらない、明確に傾向があれば別だが。
    - Embedding(カテゴリ変数をいくつかの次元のベクトルに変換してくれる、NLPなど)
### 2. 実験と結果
- GBDTにおいてTarget encodingとLabel Encodingをどう使い分けるか？
    - カテゴリ変数だけからなり、実数値を予測する回帰問題を人工的に設定
    - 水準数は同じとする(Cardinalityを操作する)
    - Labelカテゴリ変わり当たるのはランダムに選んでいる
    - trget: サムとノイズを加えてものとする　サンプルは一様分布から生成
    - 100,000
- One-hot：Cardinalityが増えるほど難しくなる
- Label encoding: 若干OverfitしてOne-hotより悪くしまっている。単純なNum_leavesの場合は精度良いが
- taget encoding: Num_leavesがっ増えてもRMSEが悪くなりにくい
    - 複雑な気の場合はベターな選択、3-4ならラベルやOne-hotの方がいい
- LGBM Encoding: TEよりも複雑な気の場合はダメ、単純な気の場合はGood
- TEは収束速い、
- Label: 2回以上の切りでようやく別ラベルを分離できるが、TE: 1回の切り方で効率的にモデルができる
- 水準数200の場合：明らかにTEの方がいい

- 役に立たないCategorical Featureがある場合はどうか？

### 結論
- 相互作用は入れてないので注意
- それぞれのFeatureが一様分布している仮定を入れている
- 影響はAdditive
- 役に立たない特徴量があってもTEうまくいく（万能化はわからない）
---
## カレーちゃん Earthquakeコンペ
### 専業Kagglerの1年半
- Kaggle24時間やりたい人へ伝える
    - ストレスない人にはおすすめ
    - きっかけ：2018/2にMaster宣言
        - 奥さんにめっちゃ怒られた
    - 財務省：200h残業、多い時300h
    - 金融庁：残業時間0 -> 暇 -> kaggleをスタート　2018/6でTitanicしかやってないのに専業Kagglerへ
    - Santander: 奇跡的にソロゴールド（リークでソロゴールド）
- こうすればもっと良かったというまとめ
- まだ弱いので：12月までにGM宣言 -> ひたすら頭下げて了承を得た
    - 振り替えり：思うような成績が残せなかった：1ヶ月じゃなく3か月ずっと取り組むべきだったかも
    - 上位ソリューション含めてすべて理解すべきだった、
    - 慣れるまでは1つ長所を伸ばすといい->マージもしてもらいやすい
- 就職する
    - 1月に辞める予定
    - YouTuberになって不労所得50万作りたい
- Kaggleコミュニティ最高
- Novice向けKaggle本
### 地震コンペ
- モデリングとValidationについて色々な方法を考えた
#### 概要
- 音データから何秒後に自信が発生するか予測する
- MAE
- Train data:2列、
- 発生する直前に大きな振幅が発生するらしい
- テストデータは15万行、それぞれの波から何秒後に地震発生を予測
#### 学習法
- 15万行ごとに分割して特徴を作って学習する
- 最大値、最小値、平均、標準偏差でLGBMで1.794までいくのがBaseline
#### 鍵
- ホストの論文からデータ作っていることを利用
- Public/Privateの分け方が推測できた
#### スコア推移
- Kernelを参考に2000個特徴生成
- GroupKFold
- XGB
- targetをnp.sqrtする
- LGBM,Catは精度出ない
#### チームマージ
- 議論中に重要なDiscussion->リークに気づく
- P4677のホストの論文から使われたデータ
- ホストの論文：発生まで何秒かは予想しにくい
- 論文とTrainデータ少し違う、他は完全に一致->TestのTargetの分布がｗかある->2624個の中からどれがテストデータなのか当てたい
- Publicに使われているデータの位置を特定する:平均4.017, 最大値9-10
    - ランダムにサンプリングしてくるとほとんど4付近にならない
      ->どこかを一連で選んでいる？
      ->端がPublicじゃないかとおもってやってみたが良いスコア出ない
      ->候補がないので信じてモデリング
- TrainからPrivateに千秋データサンプリング
- Trainのターゲット調整->地震発生直後は10付近でよく当てられる->CVが良くなる、0に近い線を引くようなやりかたを採用
### 振り返り
- リーク前提でも楽しい勝負
- XGb単体、CATBOOST単体も提出しておけばよかった
---
## FE for Events data
- homework at home! kaggle and real jobs
### Event
- 70% are tabular data
- time-series DB on the rise
- RDBMS are not good for ML
    - Static data < changes and how they affect the goal
    - 99% churn models are done wrong
    - 電話をするかどうかを当てても意味がない->それ以降にCustomer churnかどうなるかまでチェックしないといけない
- eventss as first-class citizens for machine learning
    - Bootstrapping ->（sampling with replacement）
        - augmentationに使える,
        - ensembling
    - injecting new ones; can be used for selecting the best customer for an action
    - Checking the importance of events
        - in an iterative way
- Event とは
    - ID
    - Timestamp
    - Attributes
### Recsys 1st prize solution overview
- almost all teams use classifier to predict ranking of hotels
- trivago dataset
    - important event; click on the hovered button
    - 口コミを見たり、画像をみたりといろんなインタラクションがある
        - 画像をclickした人はきっとそのホテルが気になっているだろう
- ビジネス的意義
- metrix: MRR = 1/|Q| sum (1/ rank(i))
- Quick validation is the key= many experiments
    - Recsys challenge -> hard to overfit -> easy to test using small number of observations
    - Kaggle days SFと株って進まなかった時期もある
- Fearues / accumulators: base class
    - 未来のデータを使わずに統計量を得ることが大事
- calculate CTR example
    - ctr_corrも見る: 25 photos showed -> 5番目のホテルを撮ったら6,7,8は録らないだろうということで調べている
    - 1000くらいの小さいデータでウォーミングアップをしてみるといい
- リークアリ: time offsets vs potision offset
    - click location and timestamp があった
    - T_diff=0 -> item was the same (double clickなど)
    - T_diff <= 5; prefere to go forward most of the time
    - T_diff <= 2min; results are normal statistics 
- models -> grid search for hypterparameters
    - in the last two weeks
    - ロシア語：depth of the tree
### approaches - how to deal with time
- Event type
    - Google analyrics
    - sales events
    - newsletter tracking
    - accepted invitations
- data preparations : data augmentation
    - many snapshots of past and future -> 切り方でいろいろ水増しできる
- modeling discrete models
    - typical models use a window
    - probability of sales in the next 90 days
    - trained on discrete events like this in the past
    - easy to calculate
    - not very frexible
- Weibull time to event
    - like earthquake competition
    - flexible but not easy to calculate
    - Weibul distribution -> flexible distribution from gaussian-like, gamma-likeなど
    - 理想的にはいい形の分布が出てほしいが一様分布みたいになっちゃう(機械の生存分析なんかに使われている)
    - implement of weibull: github
    - Activation funtrionにラムダとkの2-parametersを学習できる
### tools
- FeatureTools: library to predict event data
    - adv: RDB, Deep feature synthesis
    - disadv: poor support for sparse data, quite slow however can also utilize spark
        - one-hot categorial dataにする
        - NLPのBag-of-wordsには使えない
- OR, Build your own like us
    - EventAI as a part of Fintech Accelerator
    - Based on Spark /Scala -> Fast
    - ID, Timestamp and attributes
- datasets to try ideas
    - mobile app DSB2019
    - instacart
    - EC order (brazilian ecommerce)
    - Sparkify
- homework ida
    - pick one dataset or enter DSB
    - buld a model using FeatureTools
    - Try to implement your own framework for Extracting features from time-based events
### Summary
- FeatureToolsを知ったらその他のモデルがUselessに感じるよ（Change dynamicsしなきゃいけないという独特の特徴があるから）
- Treats events like first-class citizens
- not trust static table
- make a process where you can validate ideas quickly in the minutes
    - quick validation using first 1M events, test on all events if 1M test passes
### Q&A
- ハッカソンとかできないの？（Kaggle Days）
    - FeatureToolsのことを知ってないと厳しいと思う
- チェックポイントが5つある場合はどう評価する？
    - observation day or cutoff days: 5ColumnのConsecutive columnsを作る
- few eventsへの対処は？
    - Cold-startについては何もCustomerについて分からないので難しい
