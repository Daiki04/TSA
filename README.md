# TSA (Tree-Seed Algorithm)

TSA: Tree-seed algorithm for continuous optimization（連続最適化のための木-種子アルゴリズム）

## 概要

このリポジトリは、**TSA (Tree-Seed Algorithm)** という自然界の木と種子の関係に着想を得たメタヒューリスティック最適化アルゴリズムのPython実装です。TSAは、連続最適化問題を解くための進化的アルゴリズムの一種で、木が種子を生成し、優れた種子が次世代の木となるという自然界のプロセスをモデル化しています。

## 特徴

- **自然界に着想を得たアルゴリズム**: 木が種子を散布し、環境に適応した種子が成長するプロセスを模倣
- **探索と活用のバランス**: 探索傾向パラメータ（ST）により、局所探索と大域探索のバランスを調整
- **多次元対応**: 任意の次元数の連続最適化問題に対応
- **可視化機能**: 最適化の履歴をグラフやヒートマップで可視化可能

## リポジトリの構成

```
TSA/
├── tsa.py                    # TSAアルゴリズムの本体実装
├── tsa.ipynb                 # TSAの基本的な使用例とデモ
├── atsa.ipynb                # ATSAの実装とテスト
├── tsa_jsp.ipynb             # Job Shop Problem（ジョブショップ問題）へのTSA適用例
├── jobshoplib_test.ipynb     # ジョブショップ問題のライブラリテスト
├── opfunu_test.ipynb         # opfunuライブラリ（ベンチマーク関数）のテスト
└── README.md                 # このファイル
```

## インストール

このプロジェクトを使用するには、以下の依存パッケージが必要です：

```bash
pip install numpy matplotlib opfunu
```

## 使用方法

### 基本的な使用例

```python
from tsa import TSA
from opfunu.utils import operator

# パラメータの設定
args = {
    "func": operator.sphere_func,  # 最適化する目的関数
    "n_trees": 10,                 # 木の数（探索点の数）
    "dim": 2,                      # 問題の次元数
    "lower_bound": -100,           # 探索空間の下限
    "upper_bound": 100,            # 探索空間の上限
    "st": 0.1                      # 探索傾向（0～1の値）
}

# TSAインスタンスの作成と最適化の実行
tsa = TSA(**args)
best_solution, best_fitness = tsa.optimize()

print("最適解:", best_solution)
print("最適値:", best_fitness)

# 最適化履歴の可視化
tsa.plot_history()
```

## アルゴリズムの詳細

### TSAの動作原理

1. **初期化**: 探索空間内にランダムに木（探索点）を配置
2. **種子の生成**: 各木が複数の種子を生成
   - 探索傾向（ST）の確率で最良木に向かう種子を生成（活用）
   - それ以外はランダムな木に向かう種子を生成（探索）
3. **選択と更新**: 各木について、生成された種子の中で最良のものと比較し、優れていれば木を更新
4. **収束判定**: 最大評価回数に達するか、最適値に十分近づいたら終了

### 主要パラメータ

- `n_trees`: 木の数（探索点の数）。多いほど探索能力が高まるが、計算コストも増加
- `dim`: 問題の次元数
- `lower_bound`, `upper_bound`: 探索空間の範囲
- `st`: 探索傾向（Search Tendency）。0に近いほど探索重視、1に近いほど活用重視

## ファイル説明

### tsa.py
TSAアルゴリズムの核となる実装ファイルです。以下のクラスとメソッドを含みます：

- `TSA`クラス: アルゴリズム本体
  - `initialize_trees()`: 木の初期位置を設定
  - `optimize()`: 最適化処理のメインループ
  - `generate_seeds()`: 各木から種子を生成
  - `update_trees()`: 木の位置を更新
  - `plot_history()`: 最適化履歴のプロット
  - `plot_history_2Dheatmap()`: 2次元問題の最適化経路を可視化

### Jupyter Notebook

- **tsa.ipynb**: TSAの基本的な使い方とベンチマーク関数での評価
- **atsa.ipynb**: ATSAの実装とテスト
- **tsa_jsp.ipynb**: ジョブショップスケジューリング問題へのTSA適用例
- **jobshoplib_test.ipynb**: ジョブショップ問題のライブラリテスト
- **opfunu_test.ipynb**: 最適化ベンチマーク関数ライブラリのテスト

## 適用例

TSAは以下のような最適化問題に適用できます：

- 連続関数の最小化/最大化
- 多次元パラメータの最適化
- ジョブショップスケジューリング問題
- その他の組合せ最適化問題（離散化により）

## 参考文献

Tree-Seed Algorithmは、自然界の木と種子の関係性に着想を得たメタヒューリスティック最適化アルゴリズムです。詳細については、関連する学術論文を参照してください。

## ライセンス

このプロジェクトのライセンスについては、リポジトリの管理者にお問い合わせください。

## 貢献

バグ報告や機能追加の提案は、GitHubのIssueやPull Requestを通じて歓迎します。
