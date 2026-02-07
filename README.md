# Databricks × MLflow 3 金融機関向け提案書自動生成パイプライン

SIGNATE / FDUA 金融データ活用チャレンジ向けに構築した、Databricks上で動作する金融機関向け提案書自動生成パイプラインです。

## 概要

企業の財務データ・証券情報・地域経済データを入力として、LLMによる多段分析を経て金融機関の営業担当者向け提案書(Word文書)を自動生成します。

```
企業データ → 財務分析 → 証券分析 → 地域分析 → 提案書生成 → .docx出力
```

パイプライン全体をMLflow 3のLLMOps機能群で管理しており、プロンプトのバージョン管理・トレーシング・自動評価を組み込んでいます。

## 活用しているDatabricks機能

| 機能 | 用途 |
|:---|:---|
| Foundation Model APIs | データをDatabricks外に出さないLLM呼び出し |
| MLflow Prompt Registry | 5種類のプロンプトテンプレートのバージョン管理 |
| MLflow Tracing | 多段パイプラインの実行可視化・デバッグ |
| Scorers / Evaluation | LLM-as-a-Judgeによる提案書品質の自動評価 |
| MLflow Experiment Tracking | 実験パラメータ・メトリクスの記録 |

## 前提条件

- Databricksワークスペース(Free Edition可)
- MLflow 3.1.0以上
- Foundation Model APIsへのアクセス(Pay-per-tokenエンドポイント)

## セットアップ

1. リポジトリをDatabricksワークスペースにクローン

2. ノートブック冒頭のセットアップセルで必要なライブラリがインストールされます

```python
%pip install --upgrade "mlflow[databricks]>=3.1.0" openai databricks-agents pydantic jinja2
```

3. ノートブック内の以下の変数を環境に合わせて変更してください

```python
UC_SCHEMA = "your_catalog.your_schema"  # Unity Catalogスキーマ
EXPERIMENT_NAME = "/Workspace/Users/your_email/experiment_name"
```

## ノートブック構成

| ファイル | 内容 |
|:---|:---|
| `proposal_generation_demo.py` | メインノートブック(パイプライン全体) |

## パイプラインの流れ

1. **セットアップ** - ライブラリインストール、環境変数設定
2. **プロンプト定義・登録** - 5種類のプロンプトをPrompt Registryに登録(フォールバック付き)
3. **データ取得** - 企業の財務・証券・地域データを取得
4. **財務分析** - LLMによる財務指標の解釈と課題抽出
5. **証券分析** - 有価証券情報に基づく投資提案の生成
6. **地域経済分析** - 地域特性を踏まえたビジネス環境評価
7. **提案書生成** - 3つの分析結果を統合した提案書テキストの生成
8. **Word文書出力** - Markdown→docx変換(テーブル・書式対応)
9. **評価** - LLM Judgeスコアラーによる品質評価

## Free Editionでの利用について

Databricks Free Editionでは一部機能に制限がある場合があります。本ノートブックではフォールバック機構を実装しています。

- **Prompt Registry**: 利用不可の場合、Jinja2テンプレートで直接処理
- **サーバーレスコンピュート**: Free Editionのクォータ制限に注意

## 関連リンク

- [解説記事(Qiita)](Qiitaリンクをここに挿入)
- [金融データ活用推進協会(FDUA)](https://www.fdua.org/)
- [SIGNATE](https://signate.jp/)
- [MLflow 3 GenAI機能](https://docs.databricks.com/aws/ja/mlflow3/genai/)
- [Databricks Foundation Model APIs](https://docs.databricks.com/aws/ja/machine-learning/model-serving/score-foundation-models)

## ライセンス

MIT License
