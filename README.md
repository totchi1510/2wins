# 不良品検出モデル

このリポジトリは Google Colab 上で動かす「不良品検出（二値分類）」のサンプルである。
ResNet と Vision Transformer (ViT) の2種類のモデルを用意しており、学習から推論・評価までを簡単に試せる。
評価指標は
 Recall=100%（見逃しゼロ) を条件にしたときの Precisionと1枚あたりの推論速度（p95レイテンシなど）を想定している。

****データセット構成****

Google Drive 上に以下のようにデータを置きます。

/content/drive/MyDrive/data_splits/
 ├─ train/
 │   ├─ good/
 │   └─ bad/
 └─ val/
     ├─ good/
     └─ bad/


train : val = 8 : 2（全体の8割をtrain、2割をval）を想定

good/bad でクラス分けした画像を配置してください

🛠 必要環境

Google Colab (GPU 推奨)

Python 3.10+

PyTorch / torchvision / scikit-learn / matplotlib など
※ノートブック内で自動インストールセルあり

📓 ノートブック概要
Notebook	内容
Resnet.ipynb	ResNet を使った学習・推論・評価。モデル保存、P-Rカーブ、Recall=100%時のPrecision算出、レイテンシ測定まで。
vit.ipynb	Vision Transformer (ViT) 版。基本的な処理フローは ResNet 版と同じ。
🚀 使い方（Colab）

Google Driveをマウント

from google.colab import drive
drive.mount('/content/drive')


ノートブックを開き、上から順にセルを実行

モデル学習・評価が自動で進み、最良モデルが .pt として保存されます。

評価結果を確認

混同行列、Precision-Recall カーブ、平均精度(AP)

Recall=100% 条件での Precision

1枚当たりの推論時間(p95など)

🔍 Recall=100% 条件で Precision を出す理由

不良品検出では「見逃しゼロ」が最優先です。
そこで全ての不良品を必ず拾う閾値を設定し、その状態でどれだけ誤検出が起きるか（Precision）を見ることで、実運用での「ライン停止率」や「誤報コスト」を把握できます。

⏱ 推論速度の評価

1枚ごとの推論時間を多数回計測し、p50 / p95 / p99 を出します。
特に p95 は「95%の画像はこの時間以内に処理できる」ことを示し、現場でのタクトタイム設計や処理遅延対策の指標として有用です。

🌱 学習時の前処理（例）

train: RandomResizedCrop(224) + RandomHorizontalFlip() + 正規化

val: Resize(int(224*1.15)) + CenterCrop(224) + 正規化
※ ResNet/ViT ともに ImageNet の mean/std を使用
