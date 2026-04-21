# G4460 Gachapon · 扭蛋機抽籤 App

**G4460 數位自造者 · Spring 2026 · Week 7**
**Digital Makers course at Tatung University**

---

一台做在瀏覽器裡的 8-bit 像素扭蛋機，供課堂上 6 組學生抽任務主題。
學生對著鏡頭張大嘴（像被扭蛋嚇到的表情），觸發扭蛋掉落動畫，
抽到的主題會留在記錄牆上，不會重複。

A classroom drawing tool for the **Digital Makers** course. Six groups of students
draw one task theme each by making a wide-mouthed "surprised" expression into the camera.
Drawn themes are permanently locked on a record wall.

---

## 線上試玩 · Live Demo

> https://你的帳號.github.io/g4460-gachapon/
> *(部署後取得網址)*

---

## 功能 · Features

- **六主題扭蛋**：販賣機、遊戲機台、扭蛋機、大頭貼機、自助點餐機、夾娃娃機
- **張嘴表情觸發**（face-api.js 偵測嘴巴開合比例）
- **8-bit 音效**（純 Web Audio API 合成，無音檔依賴）
- **記錄牆**：已抽主題留在畫面上，顯示組別名稱
- **localStorage 持久化**：瀏覽器關掉重開，記錄牆還在
- **備援模式**：鏡頭失敗時可手動觸發
- **Admin 面板**：連按兩次 Shift+R 呼出，清空記錄 / 全螢幕

Six machine-themed capsules. Mouth-open expression triggers the draw.
Pixel-art aesthetic matching the course's physical materials.
Record wall persists via localStorage.
Admin panel accessible via double Shift+R (reset, fullscreen).

---

## 使用方式 · Classroom Usage

1. 老師將網址做成 QR Code 或直接打開全螢幕模式
2. 每組派一位代表上台
3. 輸入組名（例如「阿公67」）→ 投入硬幣
4. 對鏡頭張大嘴，嘴巴開合進度條滿格 → 扭蛋機震動、掉蛋
5. 點一下扭蛋打開 → 主題卡出現（附 flavor text）
6. 回到記錄牆，下一組上場
7. 六組抽完，畫面轉為「六組命運都定了」

**榮譽制**：老師在講台控場，全班看一個代表做表情抽籤，儀式感是任務二的開場。

---

## 部署到 GitHub Pages · Deploy

### 方式 A：網頁上傳（最簡單）

1. 在 GitHub 建一個新 repo，名稱 `g4460-gachapon`，設為 **Public**
2. 點 `Add file → Upload files`，把 `index.html` 和 `README.md` 拖進去
3. 進入 Settings → Pages → Source 選 `main` 分支、資料夾選 `/ (root)`
4. 等 1–2 分鐘，網址出現在 Pages 設定頁：`https://你的帳號.github.io/g4460-gachapon/`

### 方式 B：命令列

```bash
cd g4460-gachapon/
git init
git add .
git commit -m "initial commit"
git branch -M main
git remote add origin https://github.com/你的帳號/g4460-gachapon.git
git push -u origin main
```

然後同方式 A 的第 3 步啟用 Pages。

### Deploy via GitHub UI

1. Create a new **public** repo named `g4460-gachapon`
2. Upload `index.html` and `README.md`
3. Settings → Pages → Source: `main` branch, root folder
4. Wait 1–2 minutes for the URL to appear

---

## 技術細節 · Technical Notes

### 相依套件 · Dependencies

- **face-api.js** (via CDN)：臉部與嘴部 landmark 偵測
- **Google Fonts**：Press Start 2P, VT323, Noto Sans TC
- 模型檔從 jsDelivr 載入（約 400KB，第一次會 cache）

All dependencies via CDN — no build step, no bundler, no npm.

### 瀏覽器需求 · Browser Requirements

- Chrome / Edge / Safari（近兩年版本）
- 需要鏡頭權限（`getUserMedia`）
- 教室投影建議：用筆電接投影機，筆電上有 webcam

**手機瀏覽器（iOS Safari / Android Chrome）可以跑**，但投影體驗最好在電腦上。

### 關鍵參數 · Tunable Constants

在 `index.html` 的 `<script>` 區開頭：

```javascript
const MOUTH_OPEN_THRESHOLD = 0.50;   // 嘴巴張開多少算通過（MAR 0.4–0.6 常見）
const MOUTH_HOLD_FRAMES = 8;         // 要連續保持幾個偵測影格（約 0.25 秒）
```

如果某組學生的嘴型偵測不靈，可以把門檻降到 0.40 或縮短 HOLD_FRAMES。

```javascript
// Tune the mouth aspect ratio threshold and hold duration here.
```

### 六個主題與 flavor text · Themes

在 `THEMES` 常數中，每個主題有 `name`、`flavor`、`color`、`iconFn`。
修改 flavor text 直接改字串即可。

To customize themes, edit the `THEMES` array in `index.html`.

### 資料保存 · Persistence

所有抽過的主題存在 `localStorage` 的 key `g4460_gachapon_state_v1`。
重開瀏覽器、重新整理頁面都不會消失。
只有 Admin 清空 / 換瀏覽器 / 換裝置 / 清除瀏覽器資料 會重置。

Each browser has its own state. If you switch devices, the record wall starts fresh.

---

## 鍵盤快速鍵 · Keyboard Shortcuts

- `Shift + R` 連按兩次：呼出 Admin 面板
- Admin 面板有「清空記錄牆」「全螢幕」按鈕

Double tap Shift+R to reveal the admin panel.

---

## 檔案結構 · File Structure

```
g4460-gachapon/
├── index.html          # 主程式（HTML + CSS + JS 全部內嵌）
└── README.md           # 本檔
```

單一 HTML 檔，沒有建置流程、沒有外部資產。想改什麼直接改 `index.html`。

Single-file HTML. No build step required.

---

## 教學脈絡 · Pedagogical Context

這個 App 是 G4460 數位自造者課程 Week 7「自己的積木自己做」的開場工具。
學生在任務一完成後，由扭蛋機指派任務二的主題。
每一組要用自己做的積木、手機（作為通電積木）做出抽到的機器。

- 任務一：六顆立方體 + 三種排列（封閉系統，撞見「我做得到」）
- 任務二：抽籤決定主題，老師補給新材料（開放系統，「我們的積木還可以長成什麼」）

扭蛋機的視覺語言（8-bit 像素、三連星隨機家族、榮譽制抽籤）本身就是任務二世界觀的一部分。
不只是工具，是序曲。

This drawing tool is the opening ritual for the course's Week 7 assignment.
Its pixel-art aesthetic deliberately matches the physical materials students are
using in class: laser-cut MDF squares and pixelated MDF shape pieces.

---

## 授權 · License

教學用途，歡迎複製改作。如果修改後想給其他老師用，麻煩在 README 保留一行
「originally by Kevin Cheng for G4460 Digital Makers」。

Free for educational use. Please retain attribution if forking for other courses.

---

## 作者 · Author

鄭穎懋 Kevin Cheng · 大同大學資訊工程系
G4460 數位自造者 · Spring 2026

Built collaboratively with Claude (Anthropic).
