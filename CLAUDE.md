# 專案：ai-vocational-research

## 研究主題
技術型高中電機電子群教師 AI 輔助教學轉型：PPM → 轉換意願 → AIPACK → IPA

## 重要檔案路徑

- 研究計畫書：`docs/研究計畫書_APA7_三研究設計.md`
- Model F 設計：`docs/ModelF_PPM_SI_AIPACK_IPA.md`
- 參考文獻：`references/references.bib`
- SmartPLS 分析：`analysis/SmartPLS分析注意事項.md`

## 當前目標
- 量表專家效度審查
- 前測（n ≥ 30）
- 正式施測（n ≥ 200）

## Model F 核心創新
- Mooring 雙重角色（干擾 + 中介）
- 促進型 vs 阻礙型繫力區分
- PPM → SI → AIPACK 完整因果鏈
- PLS-SEM + IPMA 整合分析

## 關鍵設定
- 統計工具：SmartPLS 4（非 AMOS/CB-SEM）
- 樣本目標：n ≥ 200（PLS-SEM 支援小樣本）
- Bootstrapping：5,000 subsamples（非預設 500）
- BCa CI、HTMT < 0.85、Two-stage moderation

## GDrive 同步注意事項
- git 設定 `git config core.fsmonitor false`（避免 GDrive 衝突）
- 勿在 GDrive 同步資料夾內執行 `git gc`
