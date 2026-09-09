# Formosan-AI-Reports 臺灣原住民族語 AI 計畫報告

[![Languages](https://img.shields.io/badge/Languages-16%20Ethnic%20Groups%20%2F%2042%20Dialects-blue.svg)](lang_code_map.json)
[![Base Model](https://img.shields.io/badge/Base%20Model-Whisper%20Large--v2-orange.svg)](https://github.com/openai/whisper)
[![Clean CER](https://img.shields.io/badge/Clean%20Weighted%20CER-2.12%25%20(-23.6%25)-brightgreen.svg)](reports/2026-09-08/asr_technical_report.md)
[![Noisy CER](https://img.shields.io/badge/SNR%2010dB%20CER-4.37%25%20(-15.8%25)-green.svg)](reports/2026-09-08/asr_technical_report.md)
[![Training Data](https://img.shields.io/badge/Training%20Corpus-749%20hrs%20%2F%20592k%20samples-purple.svg)](data/2026-09-08/stats-klokah.md)

本專案收錄由**意傳科技**主導、**牧仁資訊**執行之「**臺灣原住民族語 AI 計畫 (Formosan AI Project)**」歷次技術報告、聲學模型評估數據、語料統計與各語言別標準代碼。

本計畫旨在消弭臺灣原住民族語言在現代語音與人工智慧技術中的數位鴻溝，建立覆蓋全臺灣 **16 個原住民族群、共 42 個語言別** 之高品質自動語音辨識（Automatic Speech Recognition, ASR）聲學模型與評估基準。

---

## 📌 目錄 (Table of Contents)

- [計畫核心亮點](#-計畫核心亮點-key-highlights)
- [最新技術報告](#-最新技術報告-technical-reports)
- [語料庫規模與資料集架構](#-語料庫規模與資料集架構-datasets)
- [ASR 模型評估成果摘要](#-asr-模型評估成果摘要-evaluation-results)
  - [高訊噪比基準測試 (Clean Environment)](#1-高訊噪比基準測試-clean-environment)
  - [低訊噪比抗噪測試 (Low SNR 10dB under MUSAN)](#2-低訊噪比抗噪測試-low-snr-10db-under-musan)
  - [重大突破語言個案](#3-重大突破語言個案)
  - [9 大語群評估總覽](#4-9-大語群評估總覽)
- [目錄結構與導引](#-目錄結構與導引-repository-structure)
- [16 族 42 語言別代碼對照表](#-16-族-42-語言別代碼對照表-language-codes)
- [模型推論調用說明](#-模型推論調用說明-inference--usage)
- [後續規劃與未來工作](#-後續規劃與未來工作-next-steps)
- [版權與執行團隊](#-版權與執行團隊-credits)

---

## 🌟 計畫核心亮點 (Key Highlights)

1. **全 42 語言別完整覆蓋**  
   全面涵蓋行政院原住民族委員會核定之 16 族 42 個語言別（包含高瀕危與極度低資源語言），解決國際開源大模型長期缺乏臺灣南島語專屬標籤的結構性缺憾。
2. **辨識錯誤率大幅降低（加權 CER 由 2.77% 降至 2.12%）**  
   以上一期族語 AI 計畫模型為 Baseline，新模型在 42 種語言中高達 **40 種語言（95.2%）** 辨識率顯著提升，加權平均字元錯誤率（CER）相對改善達 **23.6%**（未加權 CER 降至 2.19%）。
3. **優異的零樣本抗噪強健性（SNR 10dB 加權 CER 僅 4.37%）**  
   在訓練微調階段**完全未加入任何噪音資料增強（No Data Augmentation）**的前提下，面對注入真實生活背景噪音（MUSAN, SNR = 10 dB）的嚴苛考驗，新模型錯誤率相較 Baseline 相對降低 **15.8%**（由 5.19% 壓低至 4.37%），展現扎實的聲學表徵泛化韌性。
4. **南島語系同源初始化技術（Indonesian `<|id|>` Token Initialization）**  
   擴充 42 個語言專屬 Token，並採用同屬南島語系的印尼語嵌入權重作為先驗起點，加速跨語言注意力層收斂，徹底杜絕跨語言拼寫混淆。
5. **目前全臺最完整的標準化全族語聲學資料庫**  
   整合四大權威語料庫，經過標準化取樣（24 kHz / 16-bit Mono）、靜音切分（VAD）、EBU R128 響度正規化與頻譜重建，訓練資料達 **592,962 筆、約 749 小時**。

---

## 📄 最新技術報告 (Technical Reports)

| 報告日期 | 報告標題 | 核心內容摘要 | 完整報告連結 |
| :--- | :--- | :--- | :--- |
| **2026-09-08** | **全族語語音辨識（ASR）模型微調與強健性評估報告** | 深度解析基於 Whisper Large-v2 微調之 42 語言別模型架構、資料清理流程、高訊噪比（Clean）及低訊噪比（MUSAN SNR 10dB）評估對比。 | [檢視報告 (Markdown)](reports/2026-09-08/asr_technical_report.md) |

---

## 🗄️ 語料庫規模與資料集架構 (Datasets)

本計畫統合臺灣最具權威性的四大族語語料庫，涵蓋標準單詞例句、生活對話情境及田野口述錄音：

| 語料庫來源 | 涵蓋語言數 | 總樣本數 (筆) | 總時長 (小時) | 訓練集 (筆 / 時長) | 獨立評估集 (筆 / 時長) | 詳細統計報告 |
| :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **族語 E 樂園 (`klokah`)** | 42 | 490,726 | 603.77 | 466,204 / 573.50 hrs | 24,522 / 30.27 hrs | [stats-klokah.md](data/2026-09-08/stats-klokah.md) |
| **族語辭典 (`ilrdf_dicts`)** | 16 | 97,785 | 129.42 | 97,785 / 129.42 hrs | 0 / 0.00 hrs | [stats-ilrdf_dicts.md](data/2026-09-08/stats-ilrdf_dicts.md) |
| **意傳族語 (`ithuan_formosan`)** | 3 | 14,902 | 28.72 | 14,843 / 28.60 hrs | 59 / 0.11 hrs | [stats-ithuan_formosan.md](data/2026-09-08/stats-ithuan_formosan.md) |
| **臺大族語語料庫 (`ntu_formosan_corpus`)** | 10 | 14,130 | 17.48 | 14,130 / 17.48 hrs | 0 / 0.00 hrs | [stats-ntu_formosan_corpus.md](data/2026-09-08/stats-ntu_formosan_corpus.md) |
| **總計 (Grand Total)** | **42** | **617,543** | **779.39** | **592,962 / 749.00 hrs** | **24,581 / 30.38 hrs** | **全臺最完整族語聲學語料庫** |

> **獨立評估集切分策略**：  
> 評估資料全數取自「族語 E 樂園 (`klokah`)」，針對所有教材情境類別（Extensions）以確定性隨機種子（Seed = 42）分層抽樣 **1/20 (5.0%)**，共計 **24,522 筆（30.27 小時）**，確保 42 種語言皆具備客觀公正且互不重疊的測試基準。

---

## 📊 ASR 模型評估成果摘要 (Evaluation Results)

### 1. 高訊噪比基準測試 (Clean Environment)

在標準純淨收音條件下，以上一期 Baseline 模型為對照基準：

| 評估指標 | Baseline 模型（上一期計畫） | 本期微調新模型 | 改善幅度 (絕對值) | 相對錯誤率改善率 |
| :--- | :---: | :---: | :---: | :---: |
| **加權平均 CER** | **2.77%** | **2.12%** | **-0.65%** | **+23.6%** |
| **未加權平均 CER** | **2.91%** | **2.19%** | **-0.72%** | **+24.6%** |
| **語言表現提升比率** | - | - | - | **95.2% (40 / 42 種語言提升)** |

> **註（Baseline 條件差異）**：上一期 Baseline 模型的訓練資料當初已含有目前測試集中的絕大多數教材語料（具備先驗記憶優勢），且外部語料總量甚至更多。本期新模型在嚴格未見的測試集上依然全面超越，證實泛化能力具實質突破。

---

### 2. 低訊噪比抗噪測試 (Low SNR 10dB under MUSAN)

導入國際標準 MUSAN 雜訊庫（混合音樂、背景交談與環境噪聲），合成 **SNR = 10 dB**（生活重度噪聲環境）：

| 測試環境條件 | 聲學環境特徵 | Baseline 加權 CER | 本期新模型加權 CER | 絕對改善量 (Δ CER) | 相對錯誤率改善率 |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **Clean (High SNR)** | 原音純淨無雜訊環境 | 2.77% | **2.12%** | -0.65% | **+23.6%** |
| **SNR = 10 dB** | 生活與部落現場重度雜訊干擾 | 5.19% | **4.37%** | -0.82% | **+15.8%** |

---

### 3. 重大突破語言個案

多個上一期 Baseline 模型面臨嚴重混淆或高錯誤率的極度低資源語言，在本期取得顯著進展：

- **南勢阿美語 (`ami-x-iams`)**：CER 由 **10.29% 驟降至 1.32%**（相對錯誤率降低 **87.2%**；SNR 10dB 雜訊下亦由 12.94% 壓低至 3.08%）。
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

## 📁 目錄結構與導引 (Repository Structure)

```text
Formosan-AI-Reports/
├── README.md                     # 專案總覽與導引說明 (本文件)
├── lang_code_map.json            # 42 種臺灣原住民族語標準代碼與中文名稱對照表
├── reports/                      # 各期詳細技術評估報告
│   └── 2026-09-08/
│       └── asr_technical_report.md  # 2026-09-08 ASR 模型微調與抗噪評估完整報告
└── data/                         # 歷次實驗原始數據與統計資料
    └── 2026-09-08/
        ├── results-clean.csv     # 42 語言高訊噪比 Clean 評估明細 (CER / WER)
        ├── results-noisy.csv     # 42 語言低訊噪比 (SNR 15dB, 10dB) 抗噪評估明細
        ├── stats-klokah.md       # 族語 E 樂園語料庫切分與統計數據
        ├── stats-ilrdf_dicts.md  # 族語辭典語料庫統計數據
        ├── stats-ithuan_formosan.md # 意傳族語語料庫統計數據
        └── stats-ntu_formosan_corpus.md # 臺大族語語料庫統計數據
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

本期模型採用 **Language Tag 導流架構**，為各語言設定專屬 Token 以精準鎖定拼音規則。在進行推論時，需明確指定目標語言代碼：

```python
import torch
from transformers import AutoModelForSpeechSeq2Seq, AutoProcessor, pipeline

device = "cuda:0" if torch.cuda.is_available() else "cpu"
torch_dtype = torch.float16 if torch.cuda.is_available() else torch.float32

# 載入微調模型與處理器 (以模型發布名稱為準)
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

# 執行辨識：請透過 generate_kwargs 明確指定 language 標籤
# 例如辨識海岸阿美語 ("ami-x-pswl") 或賽考利克泰雅語 ("tay-x-sql")
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

---

## 🚀 後續規劃與未來工作 (Next Steps)

1. **多任務語種識別（Multi-task LID Loss）**：在聲學特徵層加入分類頭，實現未知語種音訊之「免標籤前端自動導流」，兼顧專屬 Token 精度與免設定易用性。
2. **動態雜訊資料增強（Online Noise Augmentation）**：在訓練階段引入動態 MUSAN 與 RIR 空間混響，進一步提升極端惡劣環境（如風切、強烈回音）下的強健性。
3. **定向採樣平衡與特定音素增強**：針對下三社魯凱語（茂林、萬山、多納）及卡群布農語等特殊音韻語言進行發音採樣補充與權重補償。
4. **模型量化與邊緣端推論加速**：推動 AWQ / INT8 量化與 Whisper Small/Medium 蒸餾，以支援部落學校離線教學平板與手持裝置。

---

## 👥 版權與執行團隊 (Credits)

- **主導單位**：[意傳科技 (Voxmosa / Ithuan)](https://ithuan.tw/)
- **執行單位**：牧仁資訊 (Muren Info)
- **語料與資料來源致謝**：
  - 財團法人原住民族語言研究發展基金會 ([ILRDF](https://www.ilrdf.org.tw/))
  - [族語 E 樂園 (klokah)](https://web.klokah.tw/)
  - 國立臺灣大學語言學研究所 (NTU Formosan Corpus)
