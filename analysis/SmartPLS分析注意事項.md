---
title: SmartPLS（PLS-SEM）分析注意事項
date: 2026-06-04
tags:
  - SmartPLS
  - PLS-SEM
  - 統計分析
  - IPA
  - IPMA
  - 研究方法
---

# SmartPLS（PLS-SEM）分析注意事項

> 適用情境：樣本數有限、模型複雜、非正態資料、預測導向研究

---

## 一、樣本數問題（這是最關鍵的）

### 1.1 最低樣本數法則

| 法則 | 公式 | 本研究情境 |
|------|------|-----------|
| **10 倍法則**（傳統經驗法則） | 最大形成性構面的題數 × 10，或指向某構面的最大路徑數 × 10 | 本研究 XK 有 5 題 → 5 × 10 = **最低 50** |
| **Cohen power analysis**（較嚴謹）| 依 effect size, power, alpha, 最大路徑數計算 | 假設 medium effect (f²=0.15), power=0.8, alpha=0.05, 最多路徑指向 DV = 10 → **最低 118** |
| **McQuitty 法則**（更嚴謹）| 樣本數 > 路徑係數數量 × 10 | 預估路徑 25 條 → **最低 250** |

### 1.2 本研究的樣本規劃建議

| 模型版本 | 預估路徑數 | 最低要求（Cohen） | 安全目標 |
|----------|:---------:|:-----------------:|:--------:|
| 純量化路徑（10 IV → 1 DV） | 10 條直接路徑 | **118** | **200** |
| 合併雙路徑（含中介） | 20+ 條 | **200** | **300** |
| 含 ML 輔助分析 | — | — | **500**（ML 不適用小樣本） |

> **結論**：若僅用 SmartPLS，n ≥ 150-200 即可進行有意義的分析。

### 1.3 小樣本（n < 150）的注意事項

| 問題 | 說明 | 緩解方式 |
|------|------|----------|
| **統計檢定力不足** | 小樣本時 weak effect 不易達顯著 | 採用 bootstrapping 5,000+ subsamples |
| **不穩定** | 參數估計變異大 | 增加 bootstrap resamples（建議至少 5,000） |
| **outer loadings 膨脹** | 小樣本可能高估因素負荷量 | 檢查 cross-loadings, 確認 AVE ≥ 0.5 |
| **Hair et al. 建議** | 最低樣本應為最大反射性構面題數 × 10 | 若 XK 5 題 → 50；若最大路徑指向同一構面 = 10 條 → 100 |

---

## 二、SmartPLS 的關鍵設定

### 2.1 演算法設定（Algorithm Settings）

| 參數 | 建議值 | 說明 |
|------|--------|------|
| Weighting Scheme | **Path Weighting** | 最穩定，標準選項 |
| Max Iterations | **300** | 預設 300，若不收斂可增至 1,000 |
| Stop Criterion | **10^-7**（0.0000001） | 收斂標準，預設值即可 |
| Initial Values | **預設** | 不需改動 |

### 2.2 Bootstrapping 設定（關鍵！）

| 參數 | 建議值 | 說明 |
|------|--------|------|
| **Subsamples** | **5,000** | 至少 5,000，推薦 10,000 更穩定 |
| **Confidence Interval Method** | **BCa (Bias-Corrected and Accelerated)** | 最穩健 |
| **Test Type** | **Two-tailed** | 標準雙尾檢定 |
| **Significance Level** | **0.05** | 可加報 0.01 與 0.001 |
| **Complete Bootstrapping** | ✅ 勾選 | 確保所有參數都有 bootstrap 標準誤 |

> ⚠️ **不要用預設的 500 subsamples**——已過時，Hair et al. (2022) 建議至少 5,000。

### 2.3 Blindfolding 設定（預測力評估）

| 參數 | 建議值 | 說明 |
|------|--------|------|
| **Omission Distance (d)** | **7** | 最常用；若樣本 < 50 則用 5 |
| **Number of Iterations** | **300** | 預設即可 |

---

## 三、測量模型（Measurement Model）評估——先看這個

### 3.1 反射性構面（Reflective）評估

> 本研究的 TPACK 各構面、行為、滿意度、學習成效均為**反射性構面**

| 指標 | 門檻 | 注意 |
|------|------|------|
| **Outer Loadings** | ≥ **0.708** | 0.4-0.7 間刪除後看 AVE 與 CR 是否改善 |
| **Composite Reliability (CR)** | ≥ **0.7** | 0.6-0.7 可接受（探索性研究） |
| **Cronbach's α** | ≥ **0.7** | 與 CR 一起報 |
| **Average Variance Extracted (AVE)** | ≥ **0.5** | 收斂效度門檻 |
| **HTMT（區別效度）** | ≤ **0.85**（嚴格）或 ≤ **0.90**（寬鬆） | HTMT > 0.90 表示構面可能無法區分 |
| **Cross-loadings** | 題項在其所屬構面上的 loading 應最高 | 輔助判斷區別效度 |

### 3.2 形成性構面（Formative）評估

> 本研究無形成性構面。**不要將 XK 設為形成性**——它是反射性的。

形成性 vs 反射性的判斷原則：
```
反射性：題項 ← 構面（題項是構面的表現，刪一題不影響構面本質）
形成性：題項 → 構面（題項構成構面，刪一題改變構面內涵）
```

---

## 四、結構模型（Structural Model）評估——後看這個

### 4.1 標準報告順序

```
第一步：共線性檢查（VIF）
第二步：路徑係數（β）與顯著性（p, t, CI）
第三步：解釋力（R²）
第四步：效果量（f²）
第五步：預測力（Q², PLSpredict）
第六步：模型配適度（SRMR）
```

### 4.2 各指標門檻與說明

| 指標 | 門檻 | 意義 | 本研究範例 |
|------|------|------|-----------|
| **VIF（內模型）** | < **5**（嚴格 < 3） | 共線性診斷 | VIF > 5 → 刪除或合併構面 |
| **β（路徑係數）** | > **0.1** 才有實質意義 | 即使 p < .05，β < 0.1 也無實務價值 | 預測 XK β 約 0.3-0.4 |
| **p 值** | < **.05** | 統計顯著性 | 同時報告 95% CI (BCa) |
| **t 值** | > **1.96**（α=0.05）| 等同 p < .05 | bootstrap t 統計量 |
| **R²** | 0.25（弱）、0.50（中）、0.75（強） | 解釋力 | 預測教學成效 R² ≈ 0.3-0.5 |
| **f²** | 0.02（小）、0.15（中）、0.35（大） | 個別構面的獨特貢獻 | 僅報有顯著路徑者 |
| **Q²** | > **0** | 預測關聯性（Stone-Geisser） | Q² > 0 表示模型有預測力 |
| **SRMR** | < **0.08** | 模型配適度 | PLS-SEM 的 SRMR 門檻較 CB-SEM 寬鬆 |

### 4.3 Importance-Performance Matrix Analysis (IPMA)

> **這與本研究 IPA 概念高度相容**——SmartPLS 內建此功能！

| 項目 | 說明 |
|------|------|
| **功能** | 以「重要性」（總效果）為 X 軸、「表現度」（平均 latent variable score）為 Y 軸，繪製優先矩陣 |
| **與 Martilla & James IPA 的關係** | 概念相同，但 IPMA 的「重要性」是從 SEM 模型計算出的**總效果**，非自評重要度 |
| **誰的觀點** | IPMA 的「重要性」是**統計上的重要性**（路徑係數），非教師主觀認知 |
| **本研究的雙重 IPA** | 可同時做兩種：① 傳統 IPA（教師自評重要度 vs 表現度）② SmartPLS IPMA（統計總效果 vs 表現度） |
| **執行步驟** | 1. 跑完 PLS-SEM → 2. IPMA 功能（須先確認所有指標尺度方向一致）→ 3. 設定目標構面（如教學成效）|

#### IPMA 執行前的注意事項

| 檢查項目 | 要求 |
|----------|------|
| 所有題項尺度方向一致 | 不能有反向題，或反向題須先 re-code 為同向 |
| 所有構面為反射性 | 形成性構面不能納入 IPMA |
| 外部權重（outer weights）非負值 | 若有負值 → 檢查資料或反向編碼 |
| Latent variable scores 已儲存 | SmartPLS 需先計算並儲存 |

---

## 五、SmartPLS vs CB-SEM（AMOS）選擇

| 比較項 | SmartPLS（PLS-SEM） | CB-SEM（AMOS） |
|--------|-------------------|----------------|
| **樣本需求** | **小樣本可用**（n ≥ 50-100） | 需大樣本（n ≥ 200-400） |
| **資料分配** | **無需常態分配** | 需多變量常態 |
| **模型複雜度** | **可處理複雜模型**（多構面、多路徑） | 複雜模型需大樣本 |
| **預測目的** | **預測導向**（最大化 R²） | 驗證導向（配適度優先） |
| **收斂問題** | **幾乎不收斂** | 常有收斂問題 |
| **形成性構面** | **支援** | 需額外設定 |
| **IPMA** | **內建** | 無對應功能 |
| **測量不變性** | MICOM 三步驟 | 多群組 CFA |
| **適合本研究** | **✅ 最適合**（小樣本、複雜模型、預測目標） | ❌ 不適合（樣本限制） |

---

## 六、本研究分析的具體 SOP

### Step 1：資料前處理（SPSS / Excel）
- 遺漏值處理：若 < 5% 採 mean replacement；> 5% 採 multiple imputation
- 反向題 recode（若有）
- 極端值檢查（z-score > ±3 需檢視）
- 常態性檢查（Skewness < |2|, Kurtosis < |7|——PLS 雖無要求但仍需報）

### Step 2：建立 SmartPLS 模型
- 繪製路徑圖（10 IV + 中介 + 3 DV）
- 設定反射性構面
- 執行 PLS Algorithm

### Step 3：測量模型評估
| 子步驟 | 檢查項目 | 過關標準 |
|--------|----------|----------|
| 3a | Outer Loadings | > 0.708 |
| 3b | Composite Reliability | > 0.7 |
| 3c | AVE | > 0.5 |
| 3d | HTMT | < 0.85 / 0.90 |
| 3e | Cross-loadings | 皆在正確構面 |

> 若 3a 未過：刪除 loading < 0.4 的題項，逐步刪除後重新跑

### Step 4：結構模型評估
| 子步驟 | 檢查項目 | 分析方法 |
|--------|----------|----------|
| 4a | VIF | < 5 |
| 4b | Path coefficients | Bootstrapping 5,000 次 |
| 4c | R² | ≥ 0.25 |
| 4d | f² | ≥ 0.02（有意義的最小值） |
| 4e | Q² | Blindfolding d=7 |
| 4f | SRMR | < 0.08 |

### Step 5：進階分析
| 分析 | 方法 | SmartPLS 功能 |
|------|------|---------------|
| **中介效果** | Bootstrap CI（間接效果 ≠ 0） | Specific Indirect Effects |
| **調節效果** | 交互作用項或多群組 | PLS-MGA（多群組比較）|
| **重要性診斷** | **IPMA** | IPMA 功能 → 目標構面：教學成效 |
| **傳統 IPA** | 自評重要度 vs 表現度 | 外部計算（SPSS/Excel）|
| **預測力** | PLSpredict | PLSpredict 功能 |

### Step 6：報告撰寫（Hair et al. 2019 標準）

> 報告須包含的要素：
> 1. 樣本特性（n, 收集方式, 遺漏值處理）
> 2. 測量模型評估表（Loadings, CR, AVE, HTMT）
> 3. 區別效度矩陣（Fornell-Larcker + HTMT）
> 4. 結構模型路徑圖（含 β, R², p 值）
> 5. 路徑係數表（β, t, p, CI, f²）
> 6. 預測力指標（Q², PLSpredict 結果）
> 7. IPMA 矩陣圖（若有執行）

---

## 七、常見錯誤與避免方式

| 常見錯誤 | 後果 | 正確做法 |
|----------|------|----------|
| 預設 500 subsamples | 估計不穩定 | **至少 5,000** |
| 只看 p 值不看 β | 統計顯著但無實務意義 | β > 0.1 才有意義 |
| 沒報 HTMT | Reviewer 一定問 | HTMT 必須報告 |
| R² 高就以為模型好 | 可能 overfitting | 同時檢查 Q² 與 SRMR |
| 形成性當反射性用 | 參數估計錯誤 | 先用概念判斷題項性質 |
| 反向題未 re-code | IPMA 無法執行 | IPMA 前確認尺度一致 |
| 中介效果只看 Baron & Kenny | 統計檢定力不足 | 用 bootstrapping 間接效果 CI |
| 沒做 PLSpredict | 無法宣稱預測力 | PLSpredict 是 PLS-SEM 的優勢 |

---

## 八、建議參考文獻（方法論）

- Hair, J. F., Risher, J. J., Sarstedt, M., & Ringle, C. M. (2019). When to use and how to report the results of PLS-SEM. *European Business Review*, 31(1), 2-24.
- Hair, J. F., Hult, G. T. M., Ringle, C. M., & Sarstedt, M. (2022). *A primer on partial least squares structural equation modeling (PLS-SEM)* (3rd ed.). Sage.
- Henseler, J., Ringle, C. M., & Sarstedt, M. (2015). A new criterion for assessing discriminant validity in variance-based structural equation modeling. *Journal of the Academy of Marketing Science*, 43(1), 115-135. [HTMT 原文]
- Shmueli, G., et al. (2019). Predictive model assessment in PLS-SEM: Guidelines for using PLSpredict. *European Journal of Marketing*, 53(11), 2322-2347.
- Ringle, C. M., & Sarstedt, M. (2016). Gain more insight from your PLS-SEM results: The importance-performance map analysis. *Industrial Management & Data Systems*, 116(9), 1865-1886. [IPMA 原文]
