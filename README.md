# CSAPP × DDIA 2e 實作學習計畫

30 週、每週 8–10 小時：前 14 週 CSAPP 配 CMU 官方 Labs 打底層基本功，後 16 週 DDIA 第二版配 MIT 6.5840 與 Gossip Glomers 做分散式系統。每週都是讀一章、做一段 Lab、寫一個小東西、週末寫一篇筆記。

## 為什麼這樣排

**先 CSAPP、再 DDIA，不交錯。** CSAPP 的行程、記憶體、網路、並行是 DDIA 的地基；Proxy Lab 寫完再進分散式，6.5840 的 RPC 和並行 bug 會好懂很多。兩本同時讀，每週 8–10 小時會被切得太碎。

| 階段 | 期間 | 週數 | 主軸 |
| --- | --- | --- | --- |
| CSAPP | 2026/11/02 – 2027/02/07 | 14 | Data → Bomb → Attack → Cache → Shell → Malloc → Proxy |
| DDIA 2e | 2027/02/08 – 2027/05/30 | 16 | Gossip Glomers 暖身 → MapReduce → KV → Raft → KV Raft |

CSAPP Ch4（處理器架構）先略過，對後端面試幫助小；Ch5、Ch7 以速讀為主。6.5840 用 Go，所以第 14 週安排 Go 暖身。Raft（W24–W27）是整份計畫最難的段落，時間不夠就讓它延一兩週，不要硬趕。

## 每週節奏

1. **週一到週三 · 閱讀（約 3 小時）**：讀當週章節，做書裡的 practice problems，不要只看。
2. **週四到週五 · Lab（約 3–4 小時）**：做當週的 Lab 段落，卡住先自己 debug 30 分鐘再查資料。
3. **週六 · 小專案（約 2 小時）**：做當週的「小東西」，程式碼推上 GitHub。
4. **週日 · 筆記（約 1 小時）**：回答當週的筆記題目，用自己的話寫，講不清楚的地方就是還沒懂。

## 進度

### 第一階段：CSAPP + CMU Labs（2026/11/02–2027/02/07）

> CS:APP 第 3 版，Labs 用官網的 Self-Study Handout

- [ ] **W1 · 11/02 週 · 位元與整數**
  - 閱讀：CSAPP Ch1 系統漫遊；Ch2.1–2.3 資訊表示、整數
  - Lab：Data Lab：整數題（bitXor、isTmax、conditional 等）
  - 小專案：寫一個 `popcount` / `bitcount` 小工具，配單元測試涵蓋邊界值
  - 筆記題目：用自己的話解釋：為什麼補數讓加減法共用同一套電路？哪些情況會溢位？
- [ ] **W2 · 11/09 週 · 浮點數**
  - 閱讀：Ch2.4 浮點數
  - Lab：Data Lab：浮點題（floatScale2、floatFloat2Int 等），完成整個 Lab
  - 小專案：CLI：輸入一個 float，印出 sign / exponent / fraction 與實際值
  - 筆記題目：為什麼 0.1 + 0.2 ≠ 0.3？denormalized 數存在的意義是什麼？
- [ ] **W3 · 11/16 週 · 機器碼：控制流程**
  - 閱讀：Ch3.1–3.6 組合語言基礎、算術、條件與迴圈
  - Lab：Bomb Lab：phase 1–3（學會 `gdb` 和 `objdump -d`）
  - 小專案：同一段 C 用 `-O0` 和 `-O2` 編譯，對照組合語言差異
  - 筆記題目：畫出 if / switch / loop 對應的組語樣板，`switch` 什麼時候會變成跳轉表？
- [ ] **W4 · 11/23 週 · 機器碼：程序與資料**
  - 閱讀：Ch3.7–3.9 程序呼叫、陣列、結構
  - Lab：Bomb Lab：phase 4–6（含 secret phase 可選）
  - 小專案：手畫一次遞迴函式的 stack frame，再用 `gdb` 單步驗證
  - 筆記題目：callee-saved 與 caller-saved 暫存器的分工，以及為什麼要這樣分
- [ ] **W5 · 11/30 週 · 緩衝區溢位**
  - 閱讀：Ch3.10 指標、緩衝區溢位與防護
  - Lab：Attack Lab：code injection 3 題 + ROP 2 題
  - 小專案：寫一個有漏洞的小程式，開關 `-fstack-protector`、ASLR 觀察差異
  - 筆記題目：canary、ASLR、NX 各擋住哪種攻擊？ROP 為什麼能繞過 NX？
- [ ] **W6 · 12/07 週 · 記憶體階層**
  - 閱讀：Ch6 記憶體階層（Ch4 處理器架構可略過或之後補）
  - Lab：Cache Lab Part A：寫一個 cache 模擬器
  - 小專案：benchmark：二維陣列按列走 vs 按行走，量出時間差
  - 筆記題目：解釋 locality，並用你的 benchmark 數字當證據
- [ ] **W7 · 12/14 週 · 效能最佳化**
  - 閱讀：Ch5 速讀（5.1–5.9），重點放在 loop unrolling 與 memory aliasing
  - Lab：Cache Lab Part B：最佳化矩陣轉置的 miss 數
  - 小專案：用 `perf stat` 量自己的轉置版本 cache miss
  - 筆記題目：blocking 為什麼能降低 miss？跟資料庫的 page 有什麼共通點？
- [ ] **W8 · 12/21 週 · 連結與行程**
  - 閱讀：Ch7 連結（速讀）；Ch8.1–8.4 例外控制流、行程
  - Lab：Shell Lab：先完成 `eval`、內建指令、fork/exec
  - 小專案：小程式：fork 出 N 個子行程，父行程用 `waitpid` 回收並印 exit status
  - 筆記題目：static vs dynamic linking 的取捨；zombie 和 orphan 行程是怎麼產生的
- [ ] **W9 · 12/28 週 · 訊號**
  - 閱讀：Ch8.5–8.8 訊號、非區域跳轉
  - Lab：Shell Lab：完成 job control（ctrl-c、ctrl-z、fg、bg、jobs）
  - 小專案：故意寫出 signal handler 的 race，再用 `sigprocmask` 修好
  - 筆記題目：為什麼 handler 裡只能呼叫 async-signal-safe 函式？
- [ ] **W10 · 01/04 週 · 虛擬記憶體**
  - 閱讀：Ch9.1–9.8 位址翻譯、TLB、`mmap`
  - Lab：Malloc Lab：先做 implicit free list 版本，能過正確性測試
  - 小專案：用 `mmap` 讀大檔 vs `read()`，比較速度與 page fault 次數
  - 筆記題目：一次 load 指令從虛擬位址到拿到資料，經過了哪些步驟？
- [ ] **W11 · 01/11 週 · 動態記憶體配置**
  - 閱讀：Ch9.9–9.11 動態配置、GC 概念、常見記憶體 bug
  - Lab：Malloc Lab：改成 explicit 或 segregated free list，拉高 util 與 throughput
  - 小專案：把 allocator 的 util / throughput 做成表格，記錄每次改動的效果
  - 筆記題目：first-fit、best-fit、segregated fit 的取捨，跟 jemalloc / tcmalloc 的設計對照
- [ ] **W12 · 01/18 週 · I/O 與網路**
  - 閱讀：Ch10 系統層級 I/O；Ch11 網路程式設計
  - Lab：Proxy Lab Part I：循序版 HTTP proxy
  - 小專案：照書做 Tiny Web Server，用 `curl` 和瀏覽器測
  - 筆記題目：socket → bind → listen → accept 的流程；RIO 套件解決了什麼問題？
- [ ] **W13 · 01/25 週 · 並行程式設計**
  - 閱讀：Ch12 並行：行程、I/O 多工、執行緒、同步
  - Lab：Proxy Lab Part II + III：多執行緒 + LRU 快取
  - 小專案：把 proxy 改成 thread pool，壓測比較 thread-per-request
  - 筆記題目：thread-per-request、thread pool、event loop（epoll）各適合什麼場景？
- [ ] **W14 · 02/01 週 · 緩衝週 + Go 暖身**
  - 閱讀：回顧 CSAPP 筆記，寫一篇總結；開始 A Tour of Go
  - Lab：補完前面沒做完的 Lab
  - 小專案：用 Go 重寫 Tiny Web Server 的核心，為 6.5840 熱身
  - 筆記題目：CSAPP 總結：哪三個觀念最改變你寫程式的方式？（春節前後，進度可放慢）

### 第二階段：DDIA 2e + 6.5840（2027/02/08–2027/05/30）

> 章名依 O'Reilly 第二版目錄（2026 年出版，共 14 章）；6.5840 Lab 編號依 Spring 2026 課表

- [ ] **W15 · 02/08 週 · 架構取捨與非功能需求**
  - 閱讀：DDIA 2e Ch1 Trade-Offs in Data Systems Architecture；Ch2 Defining Nonfunctional Requirements
  - Lab：Gossip Glomers #1 Echo、#2 Unique ID Generation（熟悉 Maelstrom）
  - 小專案：替自己公司的一個服務寫出 p50 / p99 延遲與負載描述
  - 筆記題目：為什麼看 p99 而不是平均？用 Ch2 的社群時間軸案例練一次 fan-out 取捨
- [ ] **W16 · 02/15 週 · 資料模型**
  - 閱讀：Ch3 Data Models and Query Languages
  - Lab：6.5840 Lab 1 MapReduce：先讀 MapReduce 論文，寫 worker
  - 小專案：同一份資料分別用關聯式、文件式、圖模型建模，比較查詢寫法
  - 筆記題目：什麼情況選 document model？event sourcing / CQRS 解決了什麼？
- [ ] **W17 · 02/22 週 · 儲存引擎（上）**
  - 閱讀：Ch4 Storage and Retrieval：Log-Structured Storage、B-Trees、兩者比較
  - Lab：6.5840 Lab 1：完成 coordinator、容錯（worker 掛掉重派）
  - 小專案：迷你 LSM-tree KV：WAL + memtable + SSTable 落盤
  - 筆記題目：LSM-tree vs B-tree 的寫放大、讀放大、空間放大
- [ ] **W18 · 03/01 週 · 儲存引擎（下）**
  - 閱讀：Ch4 後半：分析型儲存、列式儲存、全文與向量索引
  - Lab：Gossip Glomers #3 Broadcast（3a–3c）
  - 小專案：幫 LSM-tree 加 compaction 和 Bloom filter，量讀取效能變化
  - 筆記題目：OLTP 與 OLAP 的儲存為什麼長得不一樣？
- [ ] **W19 · 03/08 週 · 編碼與演進**
  - 閱讀：Ch5 Encoding and Evolution
  - Lab：6.5840 Lab 2 Key/Value server（線性一致的單機 KV、處理掉包）
  - 小專案：同一份資料用 JSON 與 Protobuf 編碼，比大小與速度；試一次 schema 演進
  - 筆記題目：向前相容 vs 向後相容；durable execution 是什麼？
- [ ] **W20 · 03/15 週 · 複製**
  - 閱讀：Ch6 Replication
  - Lab：Gossip Glomers #3 Broadcast（3d–3e，效率與網路分割）
  - 小專案：幫迷你 KV 加上 leader-follower 非同步複製
  - 筆記題目：replication lag 會造成哪些異常？read-your-writes 怎麼做？
- [ ] **W21 · 03/22 週 · 分片**
  - 閱讀：Ch7 Sharding
  - Lab：Gossip Glomers #4 Grow-Only Counter、#5 Kafka-Style Log
  - 小專案：實作 consistent hashing ring，模擬加減節點時的資料搬移量
  - 筆記題目：key range vs hash 分片；hot spot 怎麼緩解？
- [ ] **W22 · 03/29 週 · 交易（上）**
  - 閱讀：Ch8 Transactions：ACID、弱隔離等級、snapshot isolation
  - Lab：Gossip Glomers #6 Totally-Available Transactions
  - 小專案：在迷你 KV 上實作 MVCC 版 snapshot isolation
  - 筆記題目：write skew 的例子，以及哪種隔離等級能防住它
- [ ] **W23 · 04/05 週 · 交易（下）**
  - 閱讀：Ch8 後半：serializability（serial execution、2PL、SSI）、分散式交易
  - Lab：讀 Raft 論文（extended version）前半，準備 Lab 3
  - 小專案：寫一個 2PC 模擬器，故意讓 coordinator 在不同時間點掛掉
  - 筆記題目：2PC 為什麼會卡住？跟 consensus 差在哪？
- [ ] **W24 · 04/12 週 · 分散式系統的麻煩**
  - 閱讀：Ch9 The Trouble with Distributed Systems
  - Lab：6.5840 Lab 3A Raft leader election
  - 小專案：（本週以 Lab 為主）
  - 筆記題目：unreliable network、unreliable clock、process pause 各舉一個真實事故
- [ ] **W25 · 04/19 週 · 一致性與共識（上）**
  - 閱讀：Ch10 Consistency and Consensus：線性一致性、ID 生成與邏輯時鐘
  - Lab：6.5840 Lab 3B Raft log replication
  - 小專案：（本週以 Lab 為主）
  - 筆記題目：linearizability vs serializability；Lamport clock 能保證什麼、不能保證什麼
- [ ] **W26 · 04/26 週 · 一致性與共識（下）**
  - 閱讀：Ch10 共識一節
  - Lab：6.5840 Lab 3C persistence
  - 小專案：（本週以 Lab 為主）
  - 筆記題目：用 Raft 的角度解釋 FLP、quorum 和 fencing token
- [ ] **W27 · 05/03 週 · Raft 收尾**
  - 閱讀：回顧 Ch9–10 筆記（本週不讀新章）
  - Lab：6.5840 Lab 3D log compaction / snapshot
  - 小專案：把 Raft 的 debug log 做成可視化（依 term、server 上色）
  - 筆記題目：Raft 實作踩過的三個坑，以及怎麼抓到的
- [ ] **W28 · 05/10 週 · 批次處理**
  - 閱讀：Ch11 Batch Processing
  - Lab：6.5840 Lab 4A KV Raft：在 Raft 上做 KV 服務
  - 小專案：對照自己 Lab 1 的 MapReduce，列出跟 Spark 的差異
  - 筆記題目：為什麼 MapReduce 讓位給 dataflow 引擎？
- [ ] **W29 · 05/17 週 · 串流處理**
  - 閱讀：Ch12 Stream Processing
  - Lab：6.5840 Lab 4B+C：snapshot 與重複請求處理
  - 小專案：用 Go 寫一個小型 stream processor：讀 log、做 tumbling window 計數
  - 筆記題目：exactly-once 實際上是怎麼做到的？
- [ ] **W30 · 05/24 週 · 收尾與總結**
  - 閱讀：Ch13 A Philosophy of Streaming Systems；Ch14 Doing the Right Thing
  - Lab：可選：6.5840 Lab 5 Sharded KV
  - 小專案：把 LSM KV、Raft、proxy 整理成 GitHub 作品集，各寫 README
  - 筆記題目：全書總結 + 拿一題系統設計題（例如設計 Twitter timeline）用學到的東西完整回答一次

## 資源連結

- [CS:APP3e 官方 Lab Assignments（Self-Study Handouts）](https://csapp.cs.cmu.edu/3e/labs.html)
- [CMU 15-213 課程網站](https://www.cs.cmu.edu/~213/)
- [MIT 6.5840 Distributed Systems](https://pdos.csail.mit.edu/6.824/)
- [6.5840 課表（論文與 Lab 順序）](https://pdos.csail.mit.edu/6.824/schedule.html)
- [Gossip Glomers 分散式系統挑戰](https://fly.io/dist-sys/)
- [Maelstrom（Gossip Glomers 用的測試工具）](https://github.com/jepsen-io/maelstrom)
- [DDIA 第二版介紹](https://martin.kleppmann.com/2026/03/24/designing-data-intensive-applications-2e.html)
- [DDIA 官方網站](https://dataintensive.net/)

## 資料夾結構

```
.
├── index.html           # 離線進度頁（瀏覽器直接開）
├── csapp-labs/          # CS:APP 官方 Labs（W1–W13）
│   ├── datalab/         # W1–W2
│   ├── bomblab/         # W3–W4
│   ├── attacklab/       # W5
│   ├── cachelab/        # W6–W7
│   ├── shelllab/        # W8–W9
│   ├── malloclab/       # W10–W11
│   └── proxylab/        # W12–W13
├── ddia-labs/           # 第二階段實作（W15–W30）
│   ├── gossip-glomers/  # Fly.io 分散式挑戰（Maelstrom）
│   ├── mit-6.5840/      # MapReduce、KV server、Raft、KV Raft
│   └── mini-lsm-kv/     # 每週小專案累積的迷你 LSM-tree KV
└── notes/               # 每週筆記
    ├── TEMPLATE.md      # 每週筆記模板
    ├── csapp/
    └── ddia/
```

每完成一週，就把上面對應的 `- [ ]` 改成 `- [x]`，並把 `index.html` 裡 `WEEKS` 資料該週的 `done` 改成 `true`。

**進度頁怎麼開：** `git clone` 這個 repo（或在 GitHub 上 Download ZIP）後，直接用瀏覽器打開 `index.html` 就行，不需要網路或架站。頁面上的勾選只存在本機瀏覽器，正式進度以檔案裡的 `done` 欄位為準。
