# IBM HR Analytics × LightGBM × SHAP  
## 離職リスク分析と構造的要因の可視化

本リポジトリは、**IBM HR Analytics Attrition Dataset** を用いて  
社員の離職リスクを **機械学習（LightGBM）で予測**し、  
その予測結果を **SHAP により解釈・可視化**する分析プロジェクトです。

本分析では、年齢や勤続年数といった単純な個人属性に依存するのではなく、  
**報酬・業務負荷・マネジメント接点といった「組織構造上のギャップ」**に着目し、  
施策につながる説明可能なモデル構築を目的としています。

---

## 🔍 プロジェクト概要

### 目的
- 離職を「個人の問題」ではなく **組織設計・評価構造の問題**として捉える
- 予測精度だけでなく、**なぜ離職リスクが高いのかを説明できるモデル**を構築する
- 人事施策・組織改善に接続可能な示唆を得る

### 手法
- LightGBM による二値分類（Attrition: Yes / No）
- 評価指標：ROC-AUC
- SHAP を用いた
  - 全体傾向の分析
  - 個別社員レベルの要因分析

---

## 📊 使用データセット

**IBM HR Analytics Attrition Dataset（Kaggle 公開データ）**

- レコード数：約1,470名
- 目的変数：`Attrition`（離職有無）
- 主な情報：
  - 報酬・昇給
  - 働き方（残業・出張）
  - キャリア・役職
  - 満足度・WLB 指標

🔗 データセット  
https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset  

※ 本リポジトリには CSV ファイル自体は含めていません。

---

## 🧠 特徴量設計の考え方

### 基本方針
- 年齢・収入などの **生値をそのまま使わない**
- 「ギャップ」「相対関係」「相互作用」を明示的に設計
- SHAP による **説明可能性を前提**とした特徴量設計

---

### 1. 年齢・キャリア構造
- `is_young / is_mid / is_senior`  
  → 年齢をライフステージとして表現
- `career_density`  
  → 年齢に対するキャリア密度（期待と現実のズレ）
- `age_x_joblevel`  
  → 年齢と役職の不整合を表現

---

### 2. 職種・役割の集約
- `JobRole_group`  
  → 職種名ではなく業務特性（技術・顧客対応・管理など）で集約
- `is_manager_role`  
  → 管理職特有の構造を明示

---

### 3. 報酬と評価のギャップ
- `income_per_level`
- `level_income_gap`
- `hike_x_income`
- `hike_per_income`

→ **努力と報酬の釣り合い（Effort–Reward Imbalance）**を数値化

---

### 4. ストレス・働き方構造
- `stress_proxy`  
  → WLB・環境・人間関係を統合した指標
- `high_stress_overtime`
- `travel_distance_gap`
- `overtime_distance_gap`

→ 単一要因ではなく **負荷の重なり**を捉える設計

---

### 5. マネジメント接点
- `manager_exposure_ratio`
- `young_low_support_flag`
- `stagnation_manager_lock`

→ 支援不足・関係固定化・昇進停滞といった **構造的リスク**を表現

---

## ⚙️ モデリング

- モデル：LightGBM（二値分類）
- クラス不均衡対策：`class_weight="balanced"`
- ハイパーパラメータ探索：RandomizedSearchCV
- 評価指標：ROC-AUC

---

## 📈 モデル性能

- Train ROC-AUC：約 **0.94**
- Test ROC-AUC：約 **0.83**

過学習を抑えつつ、実用的な識別性能を確保しています。

---

## 🔎 SHAPによる解釈

### 全体分析
- SHAP summary plot により、離職リスクに寄与する要因を可視化
- 報酬・業務負荷・マネジメント接点に関する特徴量が上位に出現

### 個別分析
- 任意の社員 ID を指定
- SHAP waterfall により
  - 平均的な社員との差
  - リスク要因／抑制要因
を説明可能

---

## 📌 得られた示唆

- 離職は単一要因ではなく、**複数の構造的ギャップの重なり**で発生する
- 報酬設計・働き方・マネジメント構造の見直しが重要
- SHAP により **個人単位での説明と施策検討**が可能

---

## 🎯 想定用途

- HR Analytics / People Analytics
- 離職リスク分析の実践例
- 説明可能AI（XAI）の応用
- 組織設計・人事施策検討の補助

---

## ⚠️ 免責事項（Disclaimer）

本リポジトリに含まれるコードおよび分析結果は、  
**研究・学習目的**で作成されたものです。

本コードを使用したことによって生じたいかなる損害についても、  
作成者は責任を負いません。

また、本プロジェクトは実際の人事判断や評価に  
直接利用されることを想定したものではありません。
