# 不良品検出モデル

このリポジトリは Google Colab 上で動かす「不良品検出（二値分類）」のサンプルである。
ResNet と Vision Transformer (ViT) の2種類のモデルを用意しており、学習から推論・評価までを簡単に試せる。
評価指標は
 Recall=100%（見逃しゼロ) を条件にしたときの Precisionと1枚あたりの推論速度（p95レイテンシなど）を想定している。

### データセット構成

Google Drive 上に以下のようにデータを置く。
<pre> 
/content/drive/MyDrive/data_splits/
 ├─ train/
 │   ├─ good/
 │   └─ bad/
 └─ val/
     ├─ good/
     └─ bad/
</pre>

train : val = 8 : 2（全体の8割をtrain、2割をval）を想定

### 必要環境

Google Colab (GPU 推奨)

## ノートブック概要
**Notebook	内容**

Resnet.ipynb	ResNet を使った学習・推論・評価。モデル保存、P-Rカーブ、Recall=100%時のPrecision算出、レイテンシ測定まで。

vit.ipynb	Vision Transformer (ViT) 版。基本的な処理フローは ResNet 版と同じ。

**使い方（Colab）**

Google Driveをマウント
<pre>
from google.colab import drive
drive.mount('/content/drive')
</pre>

ノートブックを開き、上から順にセルを実行

モデル学習・評価が自動で進み、最良モデルが .pt として保存される。

評価結果を確認

Recall=100% 条件での Precision

1枚当たりの推論時間(p95など)

