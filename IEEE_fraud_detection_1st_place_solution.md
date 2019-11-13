# Magic features

- 正解データの付けられ方に着目した
    - （120日以内に）不正被害の報告を受けた”アカウント”に対してのみisFlaud=1のフラグを付けた
- 不正だと判定されたクライアントは、以後のトランザクションがすべて不正とみなされる
    - Discussionにホストがコメントしているのを読むことは大事
      →クライアント単位の識別ができることが良いモデルと考えることができる
- 過学習しないために、ユーザを特定できてしまう特徴量は使わないようにする
  - UID も、 Privateの68.2% の顧客がTraining中に現れていないので使わない
  - new_features = df.groupby('uid')[CM_columns].agg(['mean'])　としてuid自体は削除
---
# モデルについて

- 最終的なモデルは以下3つのアンサンブル
  - CatBoost (Public/Private LB of 0.9639/0.9408), 
  - LGBM (0.9617/0.9384), 
  - XGB (0.9602/0.9324)
  - NNはLBでLB 0.9432だったので採用しなかった
  - CAT and XGBの出力をLGBMに食わせるスタッキング
- 各クライアントに対して予測の平均値を用いることでLBで0.001向上した（PostPocessing）
- UIDはXGBoostで見つけた
  - あくまでバリデーションやPostPocessingのために使っており、モデルには突っ込まない
---
# EDA

- カラム数が多く匿名のため、愚直にやるのは難しい
- 頭150個は公開カーネルから、残り300は自分たちでやった
- V列はNaNに規則性があったため以下のどれかを使って列数を減らした
  - 各グループにPCA
  - 相関のない列同士を最大数まで使う
  - 平均値を使って1列にまとめあげてしまう
    V列で相関しているもの（r > 0.75）をすべて1列で置き換える.
---
# 特徴量選択

- V322-V339 は「time consistency」がなく削除
- 最終的にXGBは 250 features 、6 folds in 10 minutes. 
- 以下を実行
  - forward feature selection (using single or groups of features)
    - Feature Importanceを使う（？）
  - recursive feature elimination (using single or groups of features)
    - 
  - permutation importance
    →わかる（ランダムに値を変えてどれだけ結果が悪くなるかを調べる）
  - adversarial validation
    →わかる（TrainとTestの類似性を調べる）
  - correlation analysis
    →わかる
  - time consistency
    → single model、single feature で最初の月でTrainして、Trainの中の最後の月の isFraud を当てるタスクを解かせることで、特徴量の時間に対する一貫性を確認する。
    → training AUC 0.60、validation AUC 0.40程度の悪い特徴量を5%くらい発見して削除
    →相互作用も考慮しないといけないので他のテストの結果ももちろん加味（？）
  - client consistency（？）
    →同じことをUIDを当てるタスクで確認した？もしくはisFraudの0→1の切り替わり？
  - train/test distribution analysis
    →わかる
---
# Validation Strategy

- 1つのバリデーションだけの結果は信じないようにした
  - Train on first 4 months , skip a month, predict last month. 
  - We also did train 2, skip 2, predict 2. 
  - We did train 1 skip 4 predict 1. 
  - LBに出すスコアは train 6, skip 1, predict 1
- GroupKFold using month as the group
- UIDsを当てる精度でもモデルの良しあしを分析した
  - UIDのKnown, Unknown, questionableを当てるのがそれぞれXGB, LGBM, CATであり、
    アンサンブル/スタッキングしたら3パターンともを精度よく当てれるようになった
