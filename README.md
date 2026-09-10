# Formosan-AI-Reports 臺灣原住民族語 AI 計畫報告

[![Languages](https://img.shields.io/badge/Languages-16%20Ethnic%20Groups%20%2F%2042%20Dialects-blue.svg)](lang_code_map.json)
[![ASR Model](https://img.shields.io/badge/ASR%20Model-Whisper%20Large--v2-orange.svg)](reports/2026-09-08/asr_technical_report.md)
[![TTS Model](https://img.shields.io/badge/TTS%20Model-Higgs--TTS--3--4B%20(Boson%20AI)-red.svg)](reports/2026-09-08/tts_research_report.md)
[![Clean CER](https://img.shields.io/badge/Clean%20Weighted%20CER-2.12%25%20(-23.6%25)-brightgreen.svg)](reports/2026-09-08/asr_technical_report.md)
[![Training Corpus](https://img.shields.io/badge/Training%20Corpus-781%2B%20hrs%20%2F%20611k%20samples-purple.svg)](data/2026-09-08/stats-klokah.md)

本專案收錄由**意傳科技**主導、**牧仁資訊**執行之「**臺灣原住民族語 AI 計畫 (Formosan AI Project)**」歷次技術報告、聲學模型評估數據、語料統計與各語言別標準代碼。

本計畫旨在消弭臺灣原住民族語言在現代語音與人工智慧技術中的數位鴻溝，建立覆蓋全臺灣 **16 個原住民族群、共 42 個語言別** 之高品質**自動語音辨識（ASR）**與**語音合成（TTS）**聲學模型、評估基準及主權 AI 解決方案。

---

## 📌 目錄 (Table of Contents)

- [計畫核心亮點](#-計畫核心亮點-key-highlights)
- [最新技術報告](#-最新技術報告-technical-reports)
- [語料庫規模與資料集架構](#-語料庫規模與資料集架構-datasets)
- [ASR 模型評估成果摘要](#-asr-模型評估成果摘要-asr-benchmark)
  - [高訊噪比基準測試 (Clean Environment)](#1-高訊噪比基準測試-clean-environment)
  - [低訊噪比抗噪測試 (Low SNR 10dB under MUSAN)](#2-低訊噪比抗噪測試-low-snr-10db-under-musan)
  - [重大突破語言個案](#3-重大突破語言個案)
  - [9 大語群評估總覽](#4-9-大語群評估總覽)
- [TTS 語音合成研究成果摘要](#-tts-語音合成研究成果摘要-tts-benchmark)
  - [非紅產業鏈選型與微調架構](#1-非紅產業鏈選型與微調架構)
  - [客觀聲學指標對照 (Baseline vs. F5-TTS vs. Higgs-TTS)](#2-客觀聲學指標對照)
  - [表現力保留度實測結論 (Emotion / Prosody / SFX)](#3-表現力保留度實測結論)
- [目錄結構與導引](#-目錄結構與導引-repository-structure)
- [16 族 42 語言別代碼對照表](#-16-族-42-語言別代碼對照表-language-codes)
- [模型推論調用說明](#-模型推論調用說明-inference--usage)
- [後續規劃與未來工作](#-後續規劃與未來工作-next-steps)
- [版權與執行團隊](#-版權與執行團隊-credits)

---

## 🌟 計畫核心亮點 (Key Highlights)

1. **全 42 語言別完整覆蓋**  
   全面涵蓋行政院原住民族委員會核定之 16 族 42 個語言別（包含高瀕危與極度低資源語言），解決國際開源大模型長期缺乏臺灣南島語專屬標籤的結構性缺憾。
2. **ASR 辨識錯誤率大幅降低（加權 CER 由 2.77% 降至 2.12%）**  
   以 Baseline 模型為對照，新微調 Whisper Large-v2 在 42 種語言中高達 **40 種語言（95.2%）** 辨識率顯著提升，加權平均字元錯誤率（CER）相對改善達 **23.6%**（未加權 CER 降至 2.19%）。
3. **優異的零樣本抗噪強健性（SNR 10dB 加權 CER 僅 4.37%）**  
   在無噪音資料增強的前提下，面對 MUSAN（SNR = 10 dB）嚴苛生活噪聲考驗，錯誤率相較 Baseline 相對降低 **15.8%**（由 5.19% 壓低至 4.37%）。
4. **非紅產業鏈 TTS 技術路線突破（Boson AI Higgs-TTS-3-4B）**  
   率先導入非紅供應鏈旗艦自回歸語音大模型 `bosonai/higgs-tts-3-4b`，透過 LoRA 高效微調成功適配全族語合成，實證證實其 **Prosody（韻律/語速放慢與音高調節）** 具備高度保留度。
5. **目前全臺最完整的標準化全族語聲學資料庫（781+ 小時）**  
   整合五大權威語料庫（族語 E 樂園、族語辭典、意傳科技、臺大語言所、國網中心），堅持不採破壞性降噪，改以微軟 DNSMOS（$\ge 3.0$）與長度過濾確保音質純淨，保留南島語珍貴弱子音與聲門塞音。

---

## 📄 最新技術報告 (Technical Reports)

| 報告日期 | 領域類別 | 報告標題 | 核心內容摘要 | 完整報告連結 |
| :--- | :---: | :--- | :--- | :--- |
| **2026-09-08** | **TTS 語音合成** | **非紅產業鏈語音合成（TTS）模型微調可行性與表現力（Emotion / Prosody / SFX）保留度研究報告** | 測試以非紅大模型 `bosonai/higgs-tts-3-4b` 進行 LoRA 微調，結合新增國網中心語料庫，評估 42 語言發音準確度、WavLM 聲紋相似度與情緒/韻律/音效可控性保留度。 | [檢視報告 (Markdown)](reports/2026-09-08/tts_research_report.md) |
| **2026-09-08** | **ASR 語音辨識** | **全族語語音辨識（ASR）模型微調與強健性評估報告** | 深度解析基於 Whisper Large-v2 微調之 42 語言別模型架構、資料清理流程、高訊噪比（Clean）及低訊噪比（MUSAN SNR 10dB）評估對比。 | [檢視報告 (Markdown)](reports/2026-09-08/asr_technical_report.md) |

---

## 🗄️ 語料庫規模與資料集架構 (Datasets)

本計畫統合臺灣最具代表性的五大族語語料庫，涵蓋標準辭典、生活情境會話、廣播篇章與田野口述典藏：

| 語料庫來源 | 涵蓋語言數 | 總樣本數 (筆) | 總時長 (小時) | 訓練集 (筆 / 時長) | 獨立評估集 (筆 / 時長) | 詳細統計報告 |
| :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **族語 E 樂園 (`klokah`)** | 42 | 490,726 | 603.77 | 466,204 / 573.50 hrs | 24,522 / 30.27 hrs | [stats-klokah.md](data/2026-09-08/stats-klokah.md) |
| **族語辭典 (`ilrdf_dicts`)** | 16 | 97,785 | 129.42 | 97,785 / 129.42 hrs | 0 / 0.00 hrs | [stats-ilrdf_dicts.md](data/2026-09-08/stats-ilrdf_dicts.md) |
| **意傳族語 (`ithuan_formosan`)** | 3 | 14,902 | 28.72 | 14,843 / 28.60 hrs | 59 / 0.11 hrs | [stats-ithuan_formosan.md](data/2026-09-08/stats-ithuan_formosan.md) |
| **臺大族語語料庫 (`ntu_formosan_corpus`)** | 10 | 14,130 | 17.48 | 14,130 / 17.48 hrs | 0 / 0.00 hrs | [stats-ntu_formosan_corpus.md](data/2026-09-08/stats-ntu_formosan_corpus.md) |
| **國網中心族語 (`nchc_formosan`)** *(新增)* | 6 | 18,120 | 32.19 | 18,120 / 32.19 hrs | 0 / 0.00 hrs | [stats-nchc_formosan.md](data/2026-09-08/stats-nchc_formosan.md) |
| **總計 (Grand Total)** | **42** | **635,663** | **811.58** | **611,082 / 781.19 hrs** | **24,581 / 30.38 hrs** | **全臺最完整族語聲學資料庫** |

> **語音品質把關原則（不採破壞性降噪）**：  
> 為避免破壞南島語言關鍵的微弱塞音爆破音、聲門閉鎖音（`/ʔ/`）與高頻擦音，資料前處理**完全不實施破壞性濾波去噪**，改以 Microsoft DNSMOS 神經評估模型（`dnsmos_oval >= 3.0`）剔除低質錄音，並設定 `duration >= 3.0s` 過濾短促碎音。

---

## 📊 ASR 模型評估成果摘要 (ASR Benchmark)

### 1. 高訊噪比基準測試 (Clean Environment)

在標準純淨收音條件下，以上一期 Baseline 模型為對照基準：

| 評估指標 | Baseline 模型（上一期計畫） | 本期微調新模型 | 改善幅度 (絕對值) | 相對錯誤率改善率 |
| :--- | :---: | :---: | :---: | :---: |
| **加權平均 CER** | **2.77%** | **2.12%** | **-0.65%** | **+23.6%** |
| **未加權平均 CER** | **2.91%** | **2.19%** | **-0.72%** | **+24.6%** |
| **語言表現提升比率** | - | - | - | **95.2% (40 / 42 種語言提升)** |

---

### 2. 低訊噪比抗噪測試 (Low SNR 10dB under MUSAN)

導入國際標準 MUSAN 雜訊庫，合成 **SNR = 10 dB**（生活重度噪聲環境）：

| 測試環境條件 | 聲學環境特徵 | Baseline 加權 CER | 本期新模型加權 CER | 絕對改善量 (Δ CER) | 相對錯誤率改善率 |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **Clean (High SNR)** | 原音純淨無雜訊環境 | 2.77% | **2.12%** | -0.65% | **+23.6%** |
| **SNR = 10 dB** | 生活與部落現場重度雜訊干擾 | 5.19% | **4.37%** | -0.82% | **+15.8%** |

---

### 3. 重大突破語言個案

- **南勢阿美語 (`ami-x-iams`)**：CER 由 **10.29% 驟降至 1.32%**（相對降低 **87.2%**；SNR 10dB 雜訊下由 12.94% 降至 3.08%）。
- **茂林魯凱語 (`dru-x-tldr`)**：CER 由 **14.82% 壓制至 8.64%**（相對改善 **41.7%**；10dB 雜訊下由 20.22% 降至 12.80%）。
- **賽夏語 (`xsy`)**：CER 由 **5.12% 降至 2.85%**（相對改善 **44.3%**）。
- **南王卑南語 (`pyu-x-pym`)**：CER 由 **1.85% 降至 0.61%**（相對改善 **67.0%**；SNR 10dB 下僅 1.98%）。
- **東排灣語 (`pwn-x-kcdsn`)**：CER 由 **3.44% 降至 1.89%**（相對改善 **45.1%**）。
- **卡那卡那富語 (`xnb`)**：CER 由 **1.72% 降至 1.14%**（相對改善 **33.7%**）。

---

### 4. 9 大語群評估總覽

| 語群分類 | 涵蓋語言數 | 評估樣本數 | Clean Baseline CER | Clean 新模型 CER | Clean 相對改善 | SNR 10dB Baseline | SNR 10dB 新模型 | 雜訊相對改善 |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **阿美語群 (Amis)** | 5 | 2,703 | 2.98% | **1.11%** | **+62.6%** | 4.36% | **2.71%** | **+37.9%** |
| **泰雅語群 (Atayal)** | 6 | 3,301 | 2.05% | **1.63%** | **+20.1%** | 4.75% | **4.41%** | **+7.0%** |
| **排灣語群 (Paiwan)** | 4 | 2,371 | 2.69% | **2.14%** | **+20.4%** | 4.54% | **4.04%** | **+10.9%** |
| **布農語群 (Bunun)** | 5 | 2,713 | 3.28% | **2.84%** | **+13.5%** | 5.61% | **4.88%** | **+13.0%** |
| **卑南語群 (Puyuma)** | 4 | 2,339 | 1.30% | **0.80%** | **+38.2%** | 3.33% | **2.83%** | **+14.8%** |
| **魯凱語群 (Rukai)** | 6 | 3,164 | 4.81% | **3.82%** | **+20.6%** | 8.62% | **7.58%** | **+12.1%** |
| **賽德克語群 (Seediq)** | 3 | 1,704 | 2.72% | **2.67%** | **+2.1%** | 5.10% | **4.54%** | **+11.1%** |
| **太魯閣語群 (Truku)** | 1 | 670 | 1.80% | **1.86%** | -3.3% | 3.79% | **3.08%** | **+18.7%** |
| **其他臺灣原住民族語群** | 8 | 5,557 | 2.47% | **1.97%** | **+19.9%** | 4.97% | **3.97%** | **+20.0%** |

---

## 🔊 TTS 語音合成研究成果摘要 (TTS Benchmark)

### 1. 非紅產業鏈選型與微調架構
- **基底大模型**：選用美國 Boson AI 開源之旗艦對話語音大模型 [`bosonai/higgs-tts-3-4b`](https://huggingface.co/bosonai/higgs-tts-3-4b)（40 億參數自回歸 LLM + Higgs Tokenizer 8-codebook 離散聲學表徵，輸出 24 kHz 音訊）。
- **訓練框架與顯存調控**：因官方未提供微調程式，整合社群非官方工具 [`tuanh123789/Higgs-tts-3-finetune`](https://github.com/tuanh123789/Higgs-tts-3-finetune)。在 4 張 24 GB VRAM 之 NVIDIA RTX A5000 上採用 **LoRA 輕量微調**，設定 Effective Batch Size = 32，單 Epoch 耗時約 7 小時（約 6,000 steps）。

### 2. 客觀聲學指標對照
自 `klokah` 與 `ithuan_formosan` 獨立評估集中抽樣（全 42 語言，每語言 30 筆，共 1,260 筆測試樣本），以上一期 Baseline ASR 模型轉錄評估錯誤率，並以 WavLM-Large 抽取聲紋比對說話者相似度（Speaker Sim）：

| 評估系統 (System) | 模型架構類型 | 訓練狀態 | WER% | CER% | 說話者相似度 (Speaker Sim) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Ground Truth (真實音檔基準)** | 原始音訊 | 原始錄音 | **10.00%** | **2.23%** | **0.646** |
| **F5-TTS (上一期基準模型)** | Flow Matching (非自回歸) | 充分收斂 | **14.69%** | **3.81%** | **0.690** |
| **Higgs-TTS-3-4B (本期 Step 6000)** | 4B LLM 自回歸 | **僅 1 Epoch (LoRA)** | **22.26%** | **6.33%** | **0.590** |

> **說明**：4B 自回歸大模型在面對 42 種全新南島語拼音時，僅微調 1 個 Epoch（6,000 steps）仍處於音素對齊學習初期，但已建立全族語自回歸發音路徑；相比之下，F5-TTS 雖短步數收斂佳，但缺乏語言模型之對話語義與進階表現力調控空間。

### 3. 表現力保留度實測結論 (Emotion / Prosody / SFX)
1. **Emotion（情緒能力消失）❌**：因 781 小時訓練語料皆為中性教學朗讀與辭典錄音，缺乏情緒標籤，導致模型發生災難性遺忘。*解法：規劃於訓練時混入 5%～10% 帶有情緒標籤之中英文高品質語料進行多語言回放約束（Rehearsal）。*
2. **Prosody（韻律語速成功保留）⭕**：自回歸 LLM 的時間步長先驗未被破壞，聽測證實語速放慢與音高起伏調節靈敏有效，提示放慢時發音明顯變慢且音質穩定。
3. **Sound Effects（音效擬聲無法觸發）❌**：Higgs 音效機制高度依賴象聲詞（Onomatopoeia）文字標記，族語為新語言且訓練資料缺乏音效對齊標籤，零樣本無法泛化。

---

## 📁 目錄結構與導引 (Repository Structure)

```text
Formosan-AI-Reports/
├── README.md                     # 專案總覽與導引說明 (本文件)
├── lang_code_map.json            # 42 種臺灣原住民族語標準代碼與中文名稱對照表
├── reports/                      # 各期詳細技術評估報告
│   └── 2026-09-08/
│       ├── asr_technical_report.md  # 全族語 ASR 模型微調與抗噪評估完整報告
│       └── tts_research_report.md  # 非紅產業鏈 TTS 模型微調與表現力評估研究報告
└── data/                         # 歷次實驗原始數據與統計資料
    └── 2026-09-08/
        ├── results-clean.csv     # 42 語言高訊噪比 Clean 評估明細 (CER / WER)
        ├── results-noisy.csv     # 42 語言低訊噪比 (SNR 15dB, 10dB) 抗噪評估明細
        ├── stats-klokah.md       # 族語 E 樂園語料庫切分與統計數據
        ├── stats-ilrdf_dicts.md  # 族語辭典語料庫統計數據
        ├── stats-ithuan_formosan.md # 意傳族語語料庫統計數據
        ├── stats-ntu_formosan_corpus.md # 臺大族語語料庫統計數據
        └── stats-nchc_formosan.md # 國網中心族語語料庫統計數據 (新增)
```

---

## 🗺️ 16 族 42 語言別代碼對照表 (Language Codes)

本計畫統一遵循 BCP 47 擴充規範（`iso639-3-x-dialect`）建立專屬語言標籤：

<details>
<summary><b>點擊展開完整 16 族 42 語言別對照表</b></summary>

| 族群別 | 語言代碼 (Language Tag) | 中文名稱 |
| :--- | :--- | :--- |
| **阿美族 (Amis)** | `ami-x-iams` | 南勢阿美語 |
| | `ami-x-pld` | 恆春阿美語 |
| | `ami-x-pswl` | 海岸阿美語 |
| | `ami-x-skl` | 秀姑巒阿美語 |
| | `ami-x-frng` | 馬蘭阿美語 |
| **泰雅族 (Atayal)** | `tay-x-cql` | 四季泰雅語 |
| | `tay-x-kls` | 宜蘭澤敖利泰雅語 |
| | `tay-x-mtuw` | 汶水泰雅語 |
| | `tay-x-sul` | 澤敖利泰雅語 |
| | `tay-x-plngw` | 萬大泰雅語 |
| | `tay-x-sql` | 賽考利克泰雅語 |
| **排灣族 (Paiwan)** | `pwn-x-pnvn` | 中排灣語 |
| | `pwn-x-vnrn` | 北排灣語 |
| | `pwn-x-ynvl` | 南排灣語 |
| | `pwn-x-kcdsn` | 東排灣語 |
| **布農族 (Bunun)** | `bnn-x-vtn` | 丹群布農語 |
| | `bnn-x-td` | 卓群布農語 |
| | `bnn-x-bkh` | 卡群布農語 |
| | `bnn-x-bnz` | 巒群布農語 |
| | `bnn-x-isbk` | 郡群布農語 |
| **卑南族 (Puyuma)** | `pyu-x-pym` | 南王卑南語 |
| | `pyu-x-ksvk` | 建和卑南語 |
| | `pyu-x-ktrp` | 知本卑南語 |
| | `pyu-x-mkzy` | 西群卑南語 |
| **魯凱族 (Rukai)** | `dru-x-kgdv` | 多納魯凱語 |
| | `dru-x-lbw` | 大武魯凱語 |
| | `dru-x-trmk` | 東魯凱語 |
| | `dru-x-tldr` | 茂林魯凱語 |
| | `dru-x-opnh` | 萬山魯凱語 |
| | `dru-x-ngdr` | 霧臺魯凱語 |
| **賽德克族 (Seediq)** | `trv-x-tgdy` | 德固達雅賽德克語 |
| | `trv-x-trk` | 德鹿谷賽德克語 |
| | `trv-x-td` | 都達賽德克語 |
| **太魯閣族 (Truku)** | `trv-x-truku` | 太魯閣語 |
| **賽夏族 (SaySiyat)** | `xsy` | 賽夏語 |
| **鄒族 (Tsou)** | `tsu` | 鄒語 |
| **達悟族 / 雅美族 (Tao)** | `tao` | 雅美語 |
| **邵族 (Thao)** | `ssf` | 邵語 |
| **噶瑪蘭族 (Kavalan)** | `ckv` | 噶瑪蘭語 |
| **撒奇萊雅族 (Sakizaya)** | `szy` | 撒奇萊雅語 |
| **卡那卡那富族 (Kanakanavu)** | `xnb` | 卡那卡那富語 |
| **拉阿魯哇族 (Hla'alua)** | `sxr` | 拉阿魯哇語 |

> 完整 JSON 檔案可參見 [`lang_code_map.json`](lang_code_map.json)。

</details>

---

## 💻 模型推論調用說明 (Inference & Usage)

### 1. ASR 語音辨識推論範例
```python
import torch
from transformers import AutoModelForSpeechSeq2Seq, AutoProcessor, pipeline

device = "cuda:0" if torch.cuda.is_available() else "cpu"
torch_dtype = torch.float16 if torch.cuda.is_available() else torch.float32

model_id = "Voxmosa/formosan-whisper-large-v2"
model = AutoModelForSpeechSeq2Seq.from_pretrained(
    model_id, torch_dtype=torch_dtype, low_cpu_mem_usage=True
).to(device)
processor = AutoProcessor.from_pretrained(model_id)

pipe = pipeline(
    "automatic-speech-recognition",
    model=model,
    tokenizer=processor.tokenizer,
    feature_extractor=processor.feature_extractor,
    torch_dtype=torch_dtype,
    device=device,
)

# 執行辨識：需透過 generate_kwargs 明確指定 language 標籤（如 "ami-x-pswl"）
audio_file = "sample.wav"
result = pipe(
    audio_file,
    generate_kwargs={
        "language": "ami-x-pswl",
        "task": "transcribe",
    },
)
print("辨識結果：", result["text"])
```

### 2. TTS 語音合成微調模型調用範例
```python
# 基於 Higgs-TTS-3-4B LoRA 權重之合成範例概念
from transformers import AutoModelForCausalLM, AutoTokenizer

model_id = "bosonai/higgs-tts-3-4b"
lora_weights = "path/to/higgs-formosan-lora-step6000"

# 輸入族語拼音文本與韻律控制標籤
prompt_text = "<|prosody:slow|> Mikowaw kita to malipahakay a demak."
# 可指定參考音訊以進行 Zero-shot 說話者聲音克隆
```

---

## 🚀 後續規劃與未來工作 (Next Steps)

1. **ASR 多任務語種識別（Multi-task LID Loss）**：於聲學特徵層加入分類頭，實現未知語種音訊之前端自動導流。
2. **TTS 延長訓練週期（Multi-Epoch Scaling）**：將 Higgs-TTS LoRA 微調由 1 Epoch 擴充至 3～5 個 Epochs，收斂音素對齊以壓低 CER。
3. **TTS 多語言情緒回放機制（Emotion Rehearsal）**：在微調批次中混入 5%～10% 帶情緒標籤之中英文對話語料，保護情緒特徵空間不發生災難性遺忘。
4. **邊緣端部署加速**：推動 ASR 與 TTS 模型之 AWQ / INT8 量化與小型化蒸餾，支援部落教室教學平板離線運作。

---

## 👥 版權與執行團隊 (Credits)

- **主導單位**：[意傳科技 (Voxmosa / Ithuan)](https://ithuan.tw/)
- **執行單位**：牧仁資訊 (Muren Info)
- **語料與合作夥伴致謝**：
  - 財團法人原住民族語言研究發展基金會 ([ILRDF](https://www.ilrdf.org.tw/))
  - [族語 E 樂園 (klokah)](https://web.klokah.tw/)
  - 國立臺灣大學語言學研究所 (NTU Formosan Corpus)
  - 國家高速網路與計算中心 ([NCHC 國網中心](https://www.nchc.org.tw/))
