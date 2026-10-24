# api-service-island

展示名稱：**{架構示範}API 服務島**

以 Three.js 呈現虛構 API 架構的互動式 3D 城市。12 個系統網域形成相連島群，包含程序建物、日夜環境、封包機器人、路網 API 流量、可搜尋介面、手機 bottom sheet、首次教學與中英雙語。

完整目標與驗收標準請參閱 [SPEC.md](./SPEC.md)。階段 5 的確認需求來自本次開發指示；依要求未反向修改 `SPEC.md`。

## 技術概要與啟動

- 單一 `index.html`，內含 HTML、CSS 與原生 ES Modules。
- Three.js `0.160.0` 由 jsDelivr CDN 載入；需要網路連線。
- addon 僅使用 OrbitControls、CSS2DRenderer。
- 無 React、npm、Vite、建置工具、外部圖片、模型或字型。

使用 VS Code 開啟專案，安裝 **Live Server**，在 `index.html` 上按右鍵選 **Open with Live Server**。請勿直接以 `file://` 開啟。

英文介面使用：`http://127.0.0.1:5500/?lang=en`（實際連接埠依 Live Server 顯示）。未帶參數時為繁體中文。

## 操作方式

- 桌面：左鍵拖曳旋轉、滾輪縮放、右鍵平移；點島嶼、建物、封包或 API 線查看詳細資料。
- 觸控：單指旋轉、雙指縮放／平移；清單與詳細資料使用底部面板。拖曳把手可調整高度，把手按鈕也可用鍵盤展開。
- 鍵盤：方向鍵旋轉視角；Esc 依序關閉最上層對話框或面板；Tab 可操作所有控制項。
- HUD：可跳至 09:00、12:00、15:00、21:00，並切換暫停、鳥瞰、名牌、封包、API 連線與日誌。
- 「?」可隨時重開三步驟教學。

## 檔案結構

```text
api-service-island/
├─ index.html   # 完整應用程式與 DATA
├─ index.before-warm-clay.bak # 美術調整前的不可執行備份
├─ index.before-gateway-showcase.bak # 入口閘道樣板前備份
├─ index.gateway-approved.bak # 已確認入口閘道樣板備份
├─ index.four-islands-approved.bak # 本輪全城擴展前備份
├─ index.before-environment-art.bak # 海水與遠山調整前備份
├─ index.before-traffic-art.bak # 流量節奏調整前備份
├─ art-review/ # 本輪同條件前後對照與逐島近景；應用程式不載入
├─ env-review/ # 環境美術調整前後的日間、夜間及手機截圖；應用程式不載入
├─ traffic-review/ # 三時段、入口排隊截圖與約 15 秒動態預覽；應用程式不載入
├─ README.md    # 使用、進度與驗證紀錄
└─ SPEC.md      # 目標規格與驗收標準
```

替換示範內容時，編輯 `index.html` 頂端的 `DATA`：保留既有 `id` 關聯，並同步維護 `domains`、服務 `model`、`flows` 端點、`dayCycle.highLoad` 與 `english` 翻譯表。所有內容目前皆為虛構資料。

## 五階段進度

| 階段 | 實作狀態 | 摘要 |
|---|---|---|
| 1. 場景骨架 | 已完成 | 12 島、橋梁、日夜、路網、Dijkstra、標籤與錯誤處理 |
| 2. 建物與環境 | 已完成 | 50 服務模型、InstancedMesh 零件庫、家具、地形、樹、遠山與雲 |
| 3. 介面與選取 | 已完成 | 搜尋清單、三種卡片、raycast、高亮、縮圖、相機飛行與 HUD |
| 4. 封包與資料流 | 已完成，另完成流量補充 | 79 個固定池封包、10 條路網連線、時段倍率、有限排隊、badge、名牌與日誌 |
| 5. 最終整合 | 已完成 | 手機 bottom sheet、完整 reduced-motion、首訪教學、雙語、可及性與 dirty render 收尾 |

「實作完成」不代表所有裝置均已驗證；實測範圍如下。

## 溫暖黏土玩具城市美術調整

- 12 種服務原型維持 `model` 欄位驅動，改為矮胖、圓角且具差異的玩具輪廓；Gateway、資料庫、保險箱、Token、時鐘、地球儀、MQ 與巨集主機等均保留各自辨識特徵。
- 色彩改以 cream、淺 stone 與柔和 teal 為大面積基底，coral 收斂為旋鈕、鎖環、郵件格等重點；材質提高 roughness 並取消金屬感。
- 以固定美術種子只替部分主建物加入兩點眼睛；島樹改為圓團／層疊樹冠，盆栽與花瓣放大，六角頂點路燈改為蘑菇燈。
- 封包維持約 `0.35` 高與原移動邏輯，改成較寬的膠囊、較大的深色眼睛及低強度霧面玩具配件。
- 夜間沿用既定 keyframes 與 `night × 3.2` emissive 規則，增加暖色環境補光並降低光溢出最高透明度，讓非發光輪廓仍可辨識。
- `index.before-warm-clay.bak` 保存本次調整前版本；它不是入口檔，也不會由 Live Server 載入。

## 島嶼樣板擴展：授權、帳務與加密

入口閘道樣板保留於 `index.gateway-approved.bak`，其建模與廣場函式未改動。本批次暫依現有 DATA 對應三座島：授權驗證 → `identity`（身分驗證）、核心帳務 → `order`（訂單交易）、交易加密模組 → `payment`（支付清算）。原 DATA 沒有使用者提供的三個島名，**名稱對應仍待確認**；未更動原名稱、服務 id、model 或 flow 關聯。

- 三座島的暖色地面、廣場形制、植栽群聚位置與柔和配色集中在 `DATA.artDirection.islandThemes`。主建物仍由服務 `model` 對應原型，再由島嶼美術配置決定變體；沒有以服務名稱或 id 決定造型。
- 授權驗證以厚鎖身、圓拱鎖環與盾形面板呈現；核心帳務使用三層厚圓柱與環帶；交易加密採圓頂保險箱、厚門與玩具旋鈕。次要服務仍保留各自的既有模型，不複製入口閘道主建物。
- 只增加既有零件庫的 instance；路網、橋梁、相機、選取、高亮、縮圖、封包與日夜 keyframes 均未改動。

### 本批次驗證

瀏覽器實測（2026-10-24，Edge Headless `155.0.4283.62`，1440×900／DPR 1）：已檢視三座島的日間全景、島嶼與服務近景、21:00 夜景及三個主建物的 160×160 縮圖；主輪廓可辨，未見建物占據橋口。實際點擊三座主建物皆選到正確服務；入口閘道的點擊與縮圖亦正常，Console 無 error／warning。12 島、50 服務、23 橋、144 路網節點與 8 條有效 flow 路線維持不變。

程式碼檢查：相對於已確認入口閘道備份，差異僅在三島的 DATA 美術配置、主建物變體與廣場配置接線；入口閘道專屬建模及植栽函式無修改。新增零件仍經 `addPart`／InstancedMesh 與服務 owner 對應。全景 draw calls 在同一量測條件下，封包與 API 線皆關為 `173 → 173`，兩者皆開為 `187 → 187`；靜態零件批次維持 `13`。

尚未驗證：實體手機與不同 GPU 的色彩／效能、真機觸控操作，以及使用者提供的三個主題名稱與現有 DATA 島名的正式對應。390×844 模擬畫面已載入，renderer pixelRatio 上限仍為 `1.4`，不等同真機驗收。

## 全城美術完成狀態

入口閘道與首批三島維持既有造型；本輪完成其餘八島。所有新主建物均由服務 `model` 對應原型，再以 `DATA.artDirection.islandThemes` 的 `mainForm`、廣場形制與色票決定美術變體；沒有改動服務 id、座標或資料流。

| 系統網域 | 狀態 | 主輪廓 |
|---|---|---|
| 入口閘道、身分驗證、訂單交易、支付清算 | 保留已確認成果 | 玩具城門、鎖盾、帳務厚圓柱、加密保險箱 |
| 客戶中心 | 本輪完成 | 圓頂庭院服務廳與兩側小翼 |
| 商品目錄 | 本輪完成 | 暖色市集櫃台與層疊貨架冠部 |
| 庫存調度 | 本輪完成 | 三格厚實倉儲積木與橫向屋簷 |
| 物流配送 | 本輪完成 | 矮胖配送驛站、包裹標誌與側邊裝卸台 |
| 訊息通知 | 本輪完成 | 粉霧屋頂、信封正面與鐘鈴冠部 |
| 數據分析 | 本輪完成 | 觀測圓柱、環帶、圓頂與前方圖表窗 |
| 平台維運 | 本輪完成 | 雙側控制塔、平頂主廳與短訊號塔 |
| 監控觀測 | 本輪完成 | 圓眼觀測器、厚支架與前方鏡頭 |

盆地地形在島嶼周圍依 DATA 中的淨空半徑平順壓低，修正原本外緣五座島近景被山坡遮住的問題；未移動島嶼、橋梁或路網。島上家具沿用既有 InstancedMesh 批次，八座新島的小機櫃仍按鏡頭距離執行 LOD。

### 本輪對照與驗證

同條件全城畫面：日間 [修改前](./art-review/before-overview-day.png)／[修改後](./art-review/after-overview-day.png)，夜間 [修改前](./art-review/before-overview-night.png)／[修改後](./art-review/after-overview-night.png)。代表性近景：[商品目錄](./art-review/after-catalog-near.png)、[物流配送](./art-review/after-delivery-near.png)、[訊息通知](./art-review/after-notification-near.png)、[監控觀測](./art-review/after-observability-near.png)；`art-review/` 另保留其餘島嶼近景、夜間近景與手機畫面。全景對照只在截圖時暫時隱藏操作面板，未修改交付 UI；應用程式不會載入這些截圖。

瀏覽器實測：2026-10-24，Edge Headless `155.0.4283.62`、1440×900、DPR 1，`prefers-reduced-motion: reduce` 使前後畫面使用固定時間。已逐一檢視八座島的日間近景、全城 12:00／21:00、商品目錄夜間近景與 390×844 模擬手機畫面；島面、主建物、植栽與橋口可見。八座新島主建物的實際滑鼠點擊均選到正確服務，160×160 模型縮圖均產生；入口閘道控制近景亦已重看。Console 無 Runtime exception、error 或 warning。

程式碼及執行資料檢查：12 組 axial 與 50 個服務 id 唯一，50 個服務皆有有效 model 和 instance；12 島均有美術配置，主建物 model 原型與配置相符，8 條 flow 端點及 dayCycle 節點有效。橋梁 `23`、路網節點 `144`、有效路線 `8` 均未變。新島服務的高亮會變色且清除後恢復原色。全景同條件下，靜態零件批次 `13 → 13`；封包與 API 線皆關 `173 → 173` draw calls，兩者皆開 `187 → 187`。手機模擬 DPR 3 被限制為 `1.4`。

尚未驗證：實體手機／觸控、Firefox 與 Safari、其他 GPU 的色彩或效能；沒有 FPS 或記憶體實測。前後截圖中的雲與背景植被會因地形淨空改變而重排，不是像素級 A/B；相機、時間、視窗尺寸和渲染開關一致。首批三座主題名稱與現有 DATA 正式島名的對應仍待使用者明確確認。

## 海水與遠山玩具美術調整

- `DATA.environment` 集中水色、寬緩低對比波紋、淺水與柔和泡沫環、盆地噪聲／梯田強度、圓丘、背景樹色與樹形、雲色等配置；日夜光照 keyframes 保持原值。水面仍由 shader 連動霧、時間與日夜色彩，沒有鏡面或新後製。
- 地形使用較低頻的固定種子 FBM、柔和頂點色與較弱梯田量化，鏡頭前方壓低；三圈遠山改為有坡面、背向城市的層疊圓丘。背景針葉樹縮短加厚，闊葉樹採圓團樹冠；12 團雲改為不規則大小的蓬鬆群落。島嶼、橋、路網及服務／資料流關聯未改。
- 修改前版本保留於 `index.before-environment-art.bak`。同條件截圖可比較：日間 [修改前](./env-review/before-day.png)／[修改後](./env-review/after-day.png)，21:00 [修改前](./env-review/before-night.png)／[修改後](./env-review/after-night.png)，手機直式 [修改前](./env-review/before-mobile.png)／[修改後](./env-review/after-mobile.png)。截圖只供審閱，非應用程式素材。

瀏覽器實測（2026-10-24，Edge Headless `155.0.4283.62`）：1440×900／DPR 1、390×844／模擬 DPR 3，以 `prefers-reduced-motion: reduce` 固定時間與雲位置。已實際查看 12:00、21:00 全景及手機畫面；島群與橋梁未被前景地形遮住。Console／Runtime 無 error、warning 或未處理例外；手機 renderer pixelRatio 為 `1.4`。同條件全景 `renderer.info.render.calls`：修改前後均為 `179`（原先控制狀態），封包與線皆關 `173 → 173`，兩者皆開 `187 → 187`；靜態零件批次仍為 `13`。reduced-motion 且無互動時，350ms 內 render frame 增量為 `0`；切回一般動畫後 350ms 內模擬時鐘由 12:00 前進至 12:03、畫面重新繪製。

程式碼與執行資料檢查：8 條 flow 路線、23 座橋及 144 個路網節點維持不變；水、雲僅在共用模擬時間推進時更新，背景樹維持桌面 `300`／手機 `150` 且不投影。尚未驗證實體手機、不同 GPU／瀏覽器的色彩、長時間效能與真機觸控；上述手機截圖屬桌面瀏覽器尺寸模擬，不是效能實測。

## API 流量繁忙感補充

原有 8 條 flow 與 id 均保留；為讓報表與 MQ／巨集服務有真正的端點，新增 `batch-report`（訂單服務 → 報表服務）及 `mq-replay`（通知中心 → 訂單服務），共 10 條有效路線。封包池固定為 79 個，每條 4～14 個；`DATA.dayCycle.flowLoad` 決定活躍數與短批次發車間隔，不改原流的行走速度。只有 `DATA.stage4.traffic.queue.nodes` 指定的通知中心及訂單服務入口可排隊，各最多 2 個；到站後入口燈短暫亮起，近景名牌／卡片顯示等待或處理狀態。全景遠距封包以既有世界座標標籤物件顯示小圓點，不強制開啟名牌文字或 API 線。所有內容均為虛構示範流量，不是即時監控或真實 RPM。

動態預覽：[同一全景鏡頭約 14.7 秒影片](./traffic-review/opening-15s.mp4)。三時段截圖：[開盤](./traffic-review/opening.png)、[結帳](./traffic-review/checkout.png)、[夜間批次](./traffic-review/batch.png)；[入口排隊與資料流卡近景](./traffic-review/queue-near.png)、[手機模擬](./traffic-review/mobile.png)。截圖只能輔助檢視，批次出發與移動節奏請看影片或在 Live Server 中操作。

瀏覽器實測（2026-10-24，Edge Headless `155.0.4283.62`、1440×900／DPR 1；手機 390×844／模擬 DPR 3）：開盤 `browse/login` 各 10／8 個活躍封包，結帳 `checkout/charge/batch-report` 為 7／6／6，夜間 `notify/batch-report/mq-replay` 為 7／8／7，安靜區域僅 1～2 個；10 條路線均有效，沒有 route error 或 Console error／warning。夜間 4.2 秒觀察內排隊峰值為 2；實際選到等待中的封包，卡片顯示「等待處理」。15 秒同鏡頭錄影期間活躍封包 `24 → 40`，日誌 `4 → 5`，未暴增。暫停與 reduced-motion 各觀察 0.7～0.8 秒，時鐘、日誌及 render frame 計數均不變。手機 pixelRatio 上限仍為 `1.4`。

同條件全景 `renderer.info.render.calls`：原版本封包／連線皆關 `173`、皆開 `187`；本輪為 `174`／`192`，預設封包開、連線關為 `182`。增加來自共用入口燈、封包輪廓與兩條有效連線，未隨請求新建 Mesh／材質／貼圖。影片與截圖為桌面 Headless 錄製，不代表實體手機 FPS、色彩或長時間負載。前述各階段的 `8 flow／64 封包` 數字為當時歷史驗證快照；**目前交付以本節的 `10 flow／79 封包池` 為準**。

## 驗證紀錄

### 瀏覽器實測

2026-10-24，以本機 Chrome Headless `155.0.8059.85` 測試：

- 1440×900、DPR 1：中文與 `?lang=en` 均完成初始化；英文標題、搜尋、島／服務／資料流卡、50 個服務、8 條路線與 64 個封包資料正確。
- 390×844、模擬 DPR 3：renderer pixelRatio 正確限制為 `1.4`；選取後清單切換為詳細 sheet，返回清單、HUD 收合與英文長文字可操作。
- 844×390、DPR 2：畫面可操作；依明訂的 `≤800px` 門檻採桌面介面，而非 bottom sheet。
- 首訪教學三步驟、下一步／開始探索、完成記錄及「?」重新開啟正常。
- localStorage 被阻擋時，場景及教學仍可啟動，Console 無未處理錯誤。
- reduced-motion 下時間維持 `08:00`，500ms 觀察期間 render frame 計數不變；手機 DPR 上限仍正常。
- 手動暫停並等待相機 damping 穩定後，700ms 觀察期間 WebGL／CSS2D render frame 計數不變。
- 英文資料流搜尋 `payment authorization` 得到正確結果；所有 12 島、50 服務與 8 flow 可依既有共用選取流程開卡。
- CDN 封鎖時顯示「Unable to load the 3D engine」及 Reload；獨立停用 WebGL 的 Chrome 顯示「Unable to start WebGL」及可理解原因。故障注入未留在交付檔案。
- Console 未出現 error 或 warning。

效能量測條件：Chrome Headless、1440×900、DPR 1、全景、名牌及日誌關閉、相機穩定。`renderer.info.render.calls`：

| 封包 | API 線 | draw calls |
|---|---|---:|
| 關 | 關 | 174 |
| 開 | 關 | 180 |
| 關 | 開（8 條） | 182 |
| 開 | 開（8 條） | 188 |

這是桌面模擬量測，不代表實體手機 FPS 或記憶體表現。

### 美術調整瀏覽器實測

2026-10-24，以本機 Edge Headless `155.0.4283.62`、1440×900／DPR 1 與 390×844／模擬 DPR 3 檢查：

- 日間全景、API Gateway 近景／160×160 模型縮圖、21:00 夜景及手機直式畫面均完成實際截圖檢視；初始化與操作期間沒有 Runtime exception、Console error 或 warning。
- 12 島、50 服務、8 flow 與全部 model 關聯有效；`serviceInstances` 仍可由服務選取取得，服務卡與 coral 高亮的共用流程正常。
- 近景可辨識厚拱門、圓胖機櫃／建物、蘑菇燈、圓團樹冠、花盆及圓胖封包；調整後縮圖同步呈現同一套新零件。
- 服務配置與路網位置未改；全景及近景未見建物阻塞橋梁或步道，API 路線與封包仍沿既有路網顯示。
- 夜間補光後非發光建物仍有輪廓，窗光與路燈維持局部暖光；390×844 下 renderer pixelRatio 為 `1.4`，bottom sheet 仍可閱讀。
- 即時模擬 `prefers-reduced-motion: reduce` 後，偏好狀態正確生效；連續兩次量測的模擬時間與 render frame 計數皆維持不變。
- 建物／家具靜態 InstancedMesh 批次由調整前 `14` 降為 `13`。相同 1440×900 全景條件下，封包與 API 線皆關由 `174` 降為 `173` draw calls；兩者皆開由 `188` 降為 `187`，未因裝飾增加 draw call。

此處為 Headless 桌面與手機尺寸模擬，不等同實體手機的色彩、效能或觸控測試。

### 程式碼檢查

- 模組腳本通過 Node 語法檢查；所有 Three.js import 都位於動態 `import()` 的 `try/catch` 內。
- `DATA` 為 12 島、50 服務、8 flow；英文名稱、說明、酬載、角色、等級與時段均有對應，id／model／關聯未改動。
- 手機門檻、sheet 高度、DPR 上限及教學 storage key 集中於 `DATA.stage5`。
- sheet 使用 Pointer Events、pointer capture 與獨立 `pointercancel` 復原；內容區維持捲動。
- 教學的上一步／略過、原生 modal 背景 inert、Tab 焦點限制及關閉後焦點返回均已完成程式碼檢查。
- reduced-motion 停止共用模擬時間、雲、水、封包、線條與相機慣性；恢復時仍尊重手動暫停。
- 保留單一 RAF、`dt ≤ .08`、頁面隱藏短路、有限日誌與批次 DOM 更新；ResizeObserver 不改寫被觀察元素尺寸。
- 日誌清單使用 `aria-live="off"`，不高頻播報事件；狀態同時顯示文字與色點。

### 尚未驗證

- Firefox、Safari、iOS Safari 與實體低階 Android。
- 實體觸控裝置的長時間拖曳、瀏覽器工具列與螢幕旋轉行為。
- 觸控實機上的 sheet 拖曳／`pointercancel`；自動測試已涵蓋選取切換、返回與 HUD 互斥，未等同真機手勢驗證。
- 螢幕閱讀器完整流程、正式 WCAG 對比稽核。
- 實體手機 FPS、記憶體與熱節流。

## 已知限制與規格差異

- `SPEC.md` 記錄原始第 0～10 節資料不完整；階段 5 的數值與驗收條件由本次已確認指示補足，未擅自改寫 SPEC。
- 844px 寬度高於手機門檻，因此 844×390 使用桌面配置；若產品希望橫向手機仍用 sheet，需另行確認是否調高門檻。
- 標籤避讓採畫面矩形啟發式演算法；極端瀏覽器字級或輔助縮放仍需人工複驗。
- draw call 的 20～30 目標套用於建物／家具的 instanced batches；完整場景另含島岸、橋梁、地形、陰影、封包與連線。

## 展示前檢查

- [ ] 以 VS Code Live Server 開啟 `index.html`，確認網址為 `http://` 或 `https://`，不要使用 `file://`。
- [ ] 確認展示網路可連至 jsDelivr；Three.js 與兩個 addon 均由 CDN 載入。
- [ ] 確認瀏覽器硬體加速已開啟且 WebGL 可用；若載入畫面顯示 WebGL 錯誤，先重啟瀏覽器或更換展示電腦。
- [ ] 先以實際投影／分享螢幕尺寸開啟一次，確認品牌卡、清單、HUD 與日誌字級可讀。
- [ ] 清除網站的 `api-service-island:tutorial-v1` localStorage，確認正式展示時會出現首次教學；若不需要教學則事先完成或略過。
- [ ] 展示前重新載入一次，確認 Console 沒有 error／warning，時間從 08:00 開始。
- [ ] 如需英文展示，在網址加上 `?lang=en` 後重新載入。

## 三分鐘展示腳本

1. **0:00–0:35｜全景**：介紹 12 個系統網域如何以相連島群呈現；拖曳旋轉並稍微縮放，指出橋梁就是跨網域路徑。
2. **0:35–1:05｜服務**：從清單選取「支付服務」或「API Gateway」，展示模型縮圖、狀態、三個營運指標與 coral 高亮。
3. **1:05–1:40｜API 流量**：切到「呼叫關係」，選取「交易請款」，開啟 API 連線、封包與名牌；點一個封包說明其沿 Dijkstra 路網與橋梁移動。
4. **1:40–2:20｜日誌與時間**：開啟系統日誌並篩選資料流，再依序點 09:00、15:00、21:00，展示高負載節點與日夜光線變化。
5. **2:20–2:45｜視角與適應性**：切換鳥瞰後返回全景；若時間允許，縮窄視窗展示 bottom sheet 與「◷ 時間」控制。
6. **2:45–3:00｜聲明**：明確說明所有機構、服務、指標、封包與日誌都是虛構模擬，不是即時監控，也不代表真實系統效能。

## 最終驗收結果

狀態以 [SPEC.md](./SPEC.md)、各階段已確認補充要求與實際交付為準；程式碼檢查不等同瀏覽器實測。

| 驗收面向 | 狀態 | 依據 |
|---|---|---|
| 單一 HTML、Three.js 0.160.0、addon 白名單 | 符合 | 程式碼檢查：只有 `index.html` 為應用檔；無頂層靜態 import，僅動態載入 OrbitControls、CSS2DRenderer |
| DATA 驅動、固定種子、虛構內容 | 符合 | 程式碼檢查：無 `Math.random`／外部素材；配置集中於 DATA，示範內容均為虛構 |
| 12 島、50 服務、10 flow、8 dayCycle | 符合 | 程式碼與本輪瀏覽器診斷：原 8 flow 保留、另新增 2 條有效路線；axial、服務 id 與 DATA 關聯有效 |
| 六角島、23 座唯一相鄰橋、路網與 Dijkstra | 符合 | 程式碼＋瀏覽器診斷：每島 12 節點、共 144 節點，12 網域連通，現有 10 flow 路線有效 |
| 建物、背景、日夜與夜間發光 | 符合 | 瀏覽器實測：日間／夜間皆可辨識；地形、背景樹、遠山、雲與海上物件可見 |
| 選取、高亮、相機與標籤 | 符合 | 瀏覽器實測：場景拖曳／縮放、島／服務／flow 選取、縮圖、飛行、鳥瞰與 pin 正常；20 次高亮切換無累積混色 |
| 封包、API 線、名牌、日誌與時段流量 | 部分符合 | 本輪瀏覽器實測：79 封包固定池、10 路線、三時段密度與有限排隊、暫停／reduced-motion 正常；兩個模擬日長跑為補充前測試，需再以新增節奏長跑複驗 |
| 手機、雙語、鍵盤、reduced-motion | 部分符合 | Chrome 模擬尺寸與英文關鍵流程通過；實體觸控、螢幕閱讀器及 Safari／Firefox 尚未驗證 |
| 載入、CDN、WebGL 錯誤處理 | 符合 | 瀏覽器故障測試：CDN 與 WebGL 失敗皆顯示可理解錯誤及 Reload，無白畫面 |
| 效能與資源穩定性 | 部分符合 | Chrome 長跑、dirty render、資源與 draw call 通過；未量測真機 FPS、記憶體、熱節流或跨瀏覽器效能 |

### 階段 6 瀏覽器實測

測試環境：Chrome Headless `155.0.8059.85`、Windows、1440×900／DPR 1；另測 390×844／模擬 DPR 3 與英文介面。

- 完整展示順序通過：首次教學、相機手勢、選島、選服務、縮圖與指標、資料流、封包點擊、名牌、日誌篩選、四個時段、暫停／恢復、鳥瞰／全景、搜尋／清除、手機 sheet、英文及 reduced-motion 即時切換。
- 真實 canvas 拖曳與滾輪會改變相機，未產生誤選；UI 選取沒有穿透到場景。
- 390×844 下 DPR 3 被限制為 `1.4`；詳細 sheet 可展開至全螢幕，返回清單及日誌／HUD 互斥正常，沒有無法關閉的遮擋。
- reduced-motion 下 500ms 內時間與 render frame 計數不變；先手動暫停後切換 reduced-motion，解除偏好時仍保持手動暫停。
- 暫停且 damping 穩定後，700ms 內 render frame 計數保持不變。
- 背景分頁 3 秒後回到前景，只推進回前景後約 `3.03` 模擬分鐘，日誌沒有暴增或補跑事件。
- 連續主動模擬約 302 秒，相當於約 `2.10` 個模擬日；日誌於中途達到 100 筆後不再超出上限，Console 全程無 error／warning。
- 長跑前後 DOM 均未成長；場景維持 459 objects、178 meshes、23 InstancedMesh、175 geometries、167 materials。首次走近不同物件時 renderer GPU 快取由 167／4 增至 168／7；完整暖機後再次反覆操作，穩定維持 192 geometries／7 textures，判定為合理延遲上傳而非持續洩漏。
- 全景 draw calls 維持：全關 174、僅封包 180、僅 API 線 182、兩者全開 188。

### 最終尚未驗證

- VS Code Live Server 擴充本身未在自動化環境操作；以等價的本機 HTTP server 驗證回應 `200` 與完整啟動。
- Firefox、Safari、iOS Safari、實體低階 Android、實際投影設備。
- 真機觸控拖曳與 `pointercancel`、螢幕閱讀器完整流程、正式 WCAG 對比稽核。
- 真機 FPS、程序記憶體、GPU 記憶體與熱節流；本文件不推測這些數據。

目前未發現會阻礙 Chrome 桌面展示的問題。建議使用最新版 Chrome 或 Edge、開啟硬體加速、穩定網路、1440×900 左右視窗與 DPR 1～2。

## 維護規則

- 後續變更需同步更新進度與驗證紀錄。
- 只有需求變更獲確認後，才修改 `SPEC.md`；完成實作不代表規格變更。
