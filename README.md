# -# IBM HR Analytics × LightGBM × SHAP  
## 離職リスク分析と構造的要因の可視化

本リポジトリは、IBM HR Analytics Attrition Dataset を用いて  
**社員の離職リスクを機械学習で予測し、その要因を SHAP により解釈する**  
分析プロジェクトです。

単純な属性（年齢・勤続年数など）ではなく、  
**報酬・業務負荷・マネジメント接点といった「組織構造上のギャップ」** に着目した  
特徴量設計を行っています。

---

## 🔍 プロジェクト概要

- **目的**
  - 離職を「個人属性の問題」ではなく  
    **組織設計・評価構造の問題として捉える**
  - 施策につながる説明可能なモデルを構築する

- **手法**
  - LightGBM による二値分類（Attrition: Yes / No）
  - ROC-AUC を評価指標として最適化
  - SHAP による全体・個人レベルの要因分析

---

## 📊 使用データセット

**IBM HR Analytics Attrition Dataset（Kaggle）**

- 公開元  
  https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset

- ライセンス  
  Kaggle 上で公開されている **研究・学習目的での利用が可能な公開データ**  
  ※ 本リポジトリには CSV ファイル自体は含めていません

### データ取得方法

1. Kaggle にログイン
2. 上記ページから CSV をダウンロード
3. ローカル環境または Colab に配置

```bash
# 例（任意）
data/WA_Fn-UseC_-HR-Employee-Attrition.csv
