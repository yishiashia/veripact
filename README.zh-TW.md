# VeriPact

[English](README.md) | **繁體中文**

**為 coding agent 而生的「可驗證交付」。**

Coding agent 產出變更的速度，已經超過人類能逐一讀完的速度。瓶頸不再只是「寫程式」，而是「**能不能信任交付結果**」。VeriPact 是一個本機優先、以 Git 為底的控制層，目標是讓 agent 的交付變得**可驗證、可追溯、可對帳**：當初談好什麼、實際改了什麼、真正跑過哪些檢查、誰做了獨立確認，以及**誰有權**讓它繼續往下走。

它幫助人類監督**結果**，同時不把「檢查通過」誤當成「系統一定正確」，也不把「審查完成」誤當成自動取得 merge 或 deploy 的權限。

> **狀態：早期／概念。** 本 repo 目前公開的是設計與信任模型。實作正在私下開發中，尚未在此發布。歡迎 Watch 或 Star 追蹤釋出進度。

這是一個獨立的社群專案，與 GitHub、OpenAI、Anthropic 或任何 coding agent 廠商皆無隸屬關係。

## 問題

當 agent 說「做完了」，你其實接手了四個綠色勾勾無法回答的問題：

1. 它做的，真的是我們當初談好的嗎？
2. 它引用的那些檢查，真的是對**這份**程式碼跑的嗎？
3. 有人獨立確認過這個宣稱，還是我們只是選擇相信？
4. 是誰決定這可以出貨的——而一次 `git push`，是不是就悄悄變成了那個決定？

VeriPact 把這四件事拆成四個可驗證的治理關注點，並盡可能保留在 Git 與可追溯紀錄中，而不是埋在對話紀錄裡。

## 信任模型

VeriPact 建立在四個支柱上，每一個都有一篇專文深入說明。

### Contract 合約 —— *當初談好什麼*

規格、計畫、non-goals、架構決策、驗收條件與其他受治理需求，定義了 agent 該交付什麼。規格不是說完就蒸發的 prompt，而是一份具有版本基準的**合約**，讓後續變更可以和當初同意的內容對照。

→ [Spec as Contract（規格即合約）](https://yishiashia.github.io/posts/spec-as-contract/)

### Evidence 證據 —— *實際跑了什麼*

這套模型會把受治理的檢查綁定到實際被驗收的程式版本，以及產生結果時記錄的執行條件或政策。Evidence 的目標是可重現、可察覺竄改：只有能對應到「這份程式碼、這些已記錄條件」的結果，才有後續對帳的意義。

→ [Reproducible Verification and Attestation（可重現驗證與獨立確認）](https://yishiashia.github.io/posts/reproducible-verification-and-attestation/)

### Attestation 獨立確認 —— *誰確認過*

獨立審查會記錄具體發現、引用位置與需求涵蓋情況，而不是照單全收 agent 自己的「完成」宣稱。依照所採用的治理政策，核准必須處理這次審查所適用的受治理需求——涵蓋度要被檢查，而不是被假設。「獨立」指的是角色與權限上的分離，不是單純換一個模型或 agent 名稱。

→ [Reproducible Verification and Attestation（可重現驗證與獨立確認）](https://yishiashia.github.io/posts/reproducible-verification-and-attestation/)

### Authority 授權 —— *誰有權放行*

commit、push、merge、publish 與 deploy 是不同的授權動作；完成前一個動作，不會自動取得下一個動作的 Authority。依照風險與政策，放行可以由具權限的人員明確決定，也可以在事先核准的政策範圍內由自動化流程執行。自動化可以**行使被委派的 Authority**，但不會自行產生 Authority。

→ [Authority Boundary: Human on the Loop（授權邊界：Human-on-the-loop）](https://yishiashia.github.io/posts/authority-boundary-human-on-the-loop/)

## 這是 harness engineering，不是 prompt engineering

Agent 工作的可靠度，不是來自更聰明的 prompt，而是來自 agent 執行時所處的 **harness（骨架／約束環境）**：那些關卡、Evidence 綁定與 Authority 邊界，限制了 agent 能悄悄做到什麼。VeriPact 把這個 harness 本身，當成一個獨立的工程與治理問題來處理。

→ [Harness Engineering and Governance（Harness Engineering 與治理）](https://yishiashia.github.io/posts/harness-engineering-governance/)

目前 VeriPact 的設計，會用三種 workflow mode 來表達 risk-based governance：

| 模式 | 適用情境 |
| --- | --- |
| **Fast** | 小型、低風險、範圍明確，且有自動化檢查的變更 |
| **Standard** | 一般功能，或需求仍不完整的工作 |
| **Strict** | 資安、金流、隱私、權限、schema、資料遷移、公開 API 等高風險工作——使用完整關卡，並在需要時保留明確的人類決策 |

工作流程可以**升級**，但不能在沒有重新評估與理由的情況下被悄悄**降級**。每一個層級仍共用相同的可驗證底層。

Fast / Standard / Strict 是 VeriPact 的產品設計選擇，不是信任模型新增的支柱。

## 它要做什麼（以及不做什麼）

- 它的設計目標是透過 MCP **協調**既有 coding agent，並把受治理狀態保留在 Git。
- 它**不**自行提供或呼叫 LLM。
- 它**不**會因為檢查或審查通過，就自行推論出 merge 或 deploy 的 Authority。
- 它**不**宣稱 Evidence 能證明系統完全正確；它要做的是讓每個重要宣稱，都能追溯到明確的 Contract、Evidence、Attestation 與 Authority 決策。

## 藍圖（Roadmap）

- [x] 信任模型與設計
- [ ] 工作流程與關卡的公開文件
- [ ] 參考實作（MCP server + CLI）
- [ ] 範例與整合指南

## 作者

由 [yishiashia](https://yishiashia.github.io) 撰寫。上述四篇專文深入闡述了 VeriPact 背後的方法與設計思路。

## 授權（License）

本 repo 目前收錄的是**概念文件，而非已公開的原始碼。**

- 此處的**文件與文字內容**採用 [創用 CC 姓名標示 4.0 國際（CC BY 4.0）](LICENSE-DOCS) 授權。
- 未來公開的**參考實作原始碼**預計採用 [MIT License](LICENSE-CODE) 授權。
- Repository 內各類內容的授權範圍請見 [LICENSE](LICENSE)。
