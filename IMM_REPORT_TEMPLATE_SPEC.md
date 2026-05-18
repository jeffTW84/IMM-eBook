# IMM 運動科學報告模板規範
**基準報告：`ebk-1505-hs/handstand-report.html`（倒立報告）**  
整合截至 2026.05 · 適用所有後續 IMM-eBook 報告

---

## 目錄
1. [專案結構](#1-專案結構)
2. [CSS 變數系統](#2-css-變數系統)
3. [字體排版規則](#3-字體排版規則)
4. [頁面佈局](#4-頁面佈局)
5. [頂部導覽列](#5-頂部導覽列)
6. [側邊欄 Sidebar](#6-側邊欄-sidebar)
7. [Hero 區塊](#7-hero-區塊)
8. [Section 結構](#8-section-結構)
9. [元件庫（Components）](#9-元件庫)
10. [參考文獻 REF 格式](#10-參考文獻-ref-格式)
11. [Inline Citation 引用格式](#11-inline-citation-引用格式)
12. [JavaScript：Scroll Spy + 漢堡選單](#12-javascript)
13. [Footer](#13-footer)
14. [Responsive 規則](#14-responsive-規則)
15. [建立新報告 SOP](#15-建立新報告-sop)

---

## 1. 專案結構

```
important/
├── [topic]_science/          ← 參考文獻資料夾，與 Website/ 平行
│   └── references.txt
└── Website/
    └── IMM-eBook/
        ├── assets/
        │   └── imm-logo.png  ← 報告中用 ../assets/imm-logo.png
        ├── index.html        ← 總目錄，每次新增報告都要更新
        ├── ebk-XXXX-YY/     ← 每個報告一個資料夾
        │   └── [topic]-report.html
        └── IMM_REPORT_TEMPLATE_SPEC.md  ← 本文件
```

**命名規則：**
- 資料夾：`ebk-[年月]-[縮寫]`，例：`ebk-1507-sn`（2015年07月，snatch）
- HTML：`[主題]-report.html`，例：`snatch-report.html`
- references.txt：放在 `/Users/jeff/Desktop/important/[topic]_science/references.txt`

**index.html 更新：** 每新增一個報告，在 `index.html` 的 `.cat-grid` 新增一張 `.cat-card`。可用顏色：
- `--cat-accent:var(--accent)` 藍色
- `--cat-accent:var(--gold)` 金色
- `--cat-accent:var(--green)` 綠色
- `--cat-accent:var(--purple)` 紫色
- `--cat-accent:var(--teal)` 青色（teal，需在 index.html 中確認有無定義）

---

## 2. CSS 變數系統

所有報告共用同一套變數，**不得修改**。

```css
:root {
  /* 背景層次（由深到淺）*/
  --bg:      #08101e;   /* 最底層頁面背景 */
  --bg2:     #0e1929;   /* sidebar、footer 背景 */
  --surface: #111f35;   /* 表格 thead、hover 底色 */
  --card:    #162238;   /* 卡片、info-block 背景 */
  --card2:   #1a2a44;   /* 次要卡片、stage-badge 背景 */

  /* 邊框 */
  --border:  #1e3050;   /* 標準邊框 */
  --border2: #264166;   /* hover / 強調邊框 */

  /* 主色 */
  --accent:  #4f9eff;   /* 藍色：連結、section-num、ref-num、cite */
  --accent2: #7bc3ff;   /* 淺藍：h3 文字、hover 文字 */

  /* 強調色 */
  --gold:    #f5b84b;   /* 金色：h4、big-stat num、info-block.gold */
  --gold2:   #fdd888;   /* 淺金：em 文字 */
  --green:   #34d399;   /* 綠色：成功、正向指標 */
  --red:     #f87171;   /* 紅色：警告、風險 */
  --purple:  #a78bfa;   /* 紫色：特殊分類 */

  /* 文字層次 */
  --text:    #e8f1fc;   /* 主文字（近白）：標題、strong、ref-title */
  --text2:   #a8bed8;   /* 副文字：p、一般內容 */
  --text3:   #6b8aaa;   /* 弱文字：meta、label、toc-group */

  /* 字體 */
  --font:   -apple-system, BlinkMacSystemFont, "Inter", "PingFang TC", "Noto Sans TC", sans-serif;
  --serif:  "Playfair Display", "Georgia", serif;  /* h1、h2、section-title */
  --mono:   "JetBrains Mono", "Fira Code", monospace; /* 數字、代碼、ref-num */

  /* 圓角 */
  --r:   10px;  /* 標準圓角 */
  --r2:  16px;  /* 卡片大圓角 */

  /* 陰影 */
  --shadow:  0 4px 24px rgba(0,0,0,.45);
  --shadow2: 0 8px 48px rgba(0,0,0,.6);

  /* 動畫 */
  --trans: .2s cubic-bezier(.4,0,.2,1);
}
```

**報告主題色（accent-bar）：** 每份報告可依主題在 `stat-card::before` 和 hero 漸層使用不同的輔色，例：
- 抓舉 → `var(--accent)`（藍）
- 倒立 → `var(--accent)`（藍）
- Thomas Flare → `var(--accent)`（藍）
- 地雷管 → `var(--teal)`（青）

---

## 3. 字體排版規則

```css
/* 全域基礎 */
html { scroll-behavior: smooth; font-size: 16px }
body { line-height: 1.75 }

/* 標題層級 */
h2 { font-family: var(--serif); font-size: 2rem; margin-bottom: 1.5rem }
/* ↑ 用於 section-title（搭配 .section-header 使用） */

h3 { font-size: 1.2rem; color: var(--accent2);
     margin: 2rem 0 .8rem;
     padding-left: 1rem; border-left: 3px solid var(--accent);
     font-weight: 700 }
/* ↑ 藍色左邊框，用於 section 內的子標題
   命名規則：文字前加「X.Y 」副標題編號，例：「1.1 關鍵文獻」「3.2 肌群分析」
   X = section 主編號（與 section-num 一致），Y = 該 section 內順序
   ⚠️ 不可省略編號：新報告每個 h3 都必須有 X.Y 前綴 */

h4 { font-size: .85rem; color: var(--gold);
     text-transform: uppercase; letter-spacing: .08em;
     font-weight: 700; margin: 1.5rem 0 .6rem }
/* ↑ 金色大寫，用於元件內的分組標籤（insight-card、phase 標題等）
   h4 在 insight-body 內使用時需覆寫 uppercase 與 letter-spacing（見 insight-body h4 規則） */

p  { margin-bottom: 1rem; color: var(--text2) }
strong { color: var(--text); font-weight: 600 }
em { color: var(--gold2); font-style: normal; font-weight: 500 }
```

**注意：`insight-body h4` 要覆寫 h4 的 uppercase：**
```css
.insight-body h4 {
  color: var(--text); font-size: .95rem;
  text-transform: none; letter-spacing: 0;
  margin: 0 0 .35rem; font-family: var(--font)
}
```

---

## 4. 頁面佈局

```css
.page-wrap {
  display: grid;
  grid-template-columns: 260px 1fr;
  min-height: 100vh;
  max-width: 1440px;
  margin: 0 auto;
  padding-top: 56px;  /* 頂部 nav 高度 */
}

main {
  padding: 0 3rem 6rem;
  max-width: 900px;
  width: 100%;
}
```

**HTML 骨架：**
```html
<div class="page-wrap">
  <aside>...</aside>     <!-- sidebar：260px -->
  <div>                  <!-- right column -->
    <main>...</main>     <!-- 900px max-width 內容 -->
    <footer>...</footer> <!-- grid-column: 1/-1 -->
  </div>
</div>
```

---

## 5. 頂部導覽列

**固定高度 56px，position: fixed，z-index: 300**

**導覽連結固定順序（所有報告一致，不得增減）：**

| 連結文字 | 目標 URL | 備註 |
|----------|----------|------|
| 首頁 | `https://intellimotionmatrix.com/` | |
| 電子書 | `https://ebook.intellimotionmatrix.com/` | |
| Draco 裝置 | `https://draco.intellimotionmatrix.com/` | |
| Instagram | `https://www.instagram.com/dongshangyin/` | `target="_blank"` |
| 聯絡我們 | `mailto:jfiii2ofi@gmail.com` | 加 `class="cta"` |

> **⚠️ 禁止加入 `← 目錄` 連結。** `← 目錄`（`../index.html`）與「電子書」指向同一頁面，屬冗餘連結。統一以「電子書」作為返回目錄的入口，第三個連結固定為「Draco 裝置」。

```html
<nav id="topnav">
  <a class="nav-brand" href="https://ebook.intellimotionmatrix.com/">
    <img src="../assets/imm-logo.png" alt="IMM Logo">
    <div>
      <div class="nav-brand-text">智動矩陣電子書</div>
      <div class="nav-brand-sub">Breaking Science</div>
    </div>
  </a>
  <div class="nav-links">
    <a href="https://intellimotionmatrix.com/">首頁</a>
    <a href="https://ebook.intellimotionmatrix.com/">電子書</a>
    <a href="https://draco.intellimotionmatrix.com/">Draco 裝置</a>
    <a href="https://www.instagram.com/dongshangyin/" target="_blank">Instagram</a>
    <a href="mailto:jfiii2ofi@gmail.com" class="cta">聯絡我們</a>
  </div>
  <button id="ham-top" aria-label="選單">
    <span></span><span></span><span></span>
  </button>
</nav>
<div class="sidebar-backdrop" id="backdrop"></div>
```

**CSS 要點：**
```css
#topnav { position:fixed; top:0; left:0; width:100%; z-index:300;
  background:rgba(8,16,30,.92); backdrop-filter:blur(12px);
  border-bottom:1px solid var(--border);
  display:flex; align-items:center; justify-content:space-between;
  padding:.7rem 1.8rem; height:56px }
.nav-brand img { height:28px; filter:invert(1) brightness(2); opacity:.85 }
.nav-links a.cta { color:var(--accent); border-color:rgba(79,158,255,.35);
  background:rgba(79,158,255,.08) }
#ham-top { display:none } /* mobile only */
```

---

## 6. 側邊欄 Sidebar

**position: sticky，height: calc(100vh - 56px)**

```html
<aside>
  <nav>
    <div class="toc-group">概覽</div>
    <ul>
      <li><a href="#summary"><span class="toc-num">00</span>執行摘要</a></li>
    </ul>

    <div class="toc-group">核心研究</div>
    <ul>
      <li><a href="#biomechanics"><span class="toc-num">01</span>生物力學</a></li>
      <li><a href="#emg"><span class="toc-num">02</span>EMG 分析</a></li>
    </ul>

    <div class="toc-group">文獻</div>
    <ul>
      <li><a href="#references"><span class="toc-num">REF</span>參考文獻</a></li>
    </ul>
  </nav>
</aside>
```

**規則：**
- `toc-group`：群組標題（灰色、大寫）
- `toc-num`：章節編號（灰色、等寬字體，對齊用）
- 編號格式：`00`、`01`、`02`… 最後一項用 `REF`
- Active 狀態由 JS IntersectionObserver 控制（見 Section 12）

```css
aside { position:sticky; top:56px; height:calc(100vh - 56px);
  overflow-y:auto; padding:2rem 1.5rem;
  border-right:1px solid var(--border); background:var(--bg2) }
nav .toc-num { font-size:.7rem; color:var(--text3);
  font-family:var(--mono); min-width:1.4rem }
nav .toc-group { font-size:.7rem; text-transform:uppercase;
  letter-spacing:.1em; color:var(--text3);
  padding:.75rem .75rem .25rem; margin-top:.5rem }
nav a { display:flex; align-items:center; gap:.5rem;
  padding:.45rem .75rem; border-radius:6px; font-size:.82rem;
  color:var(--text2) }
nav a.active { background:var(--surface); color:var(--accent); font-weight:600 }
```

---

## 7. Hero 區塊

```html
<div class="hero">
  <div class="hero-tag">運動科學報告 · [英文副標]</div>
  <h1>[主標題（中）]<br>[副標題（中）]</h1>
  <p class="hero-sub">[一句話描述，說明報告涵蓋範圍]</p>
  <p class="hero-date">IMM-eBook · [編號] · 整合截至 [年月]</p>
  <div class="hero-stats">
    <!-- 3 個 stat-card，放最重要的 3 個數字 -->
    <div class="stat-card">
      <div class="stat-num">[數字]</div>
      <div class="stat-label">[說明]</div>
    </div>
    <div class="stat-card" style="--accent-bar:var(--gold)">...</div>
    <div class="stat-card" style="--accent-bar:var(--green)">...</div>
  </div>
</div>
```

**CSS 要點：**
```css
.hero { position:relative; padding:5rem 3rem 4rem; overflow:hidden;
  border-bottom:1px solid var(--border) }
.hero::before { content:''; position:absolute; inset:0;
  background:radial-gradient(ellipse 80% 60% at 50% -10%,
    rgba(79,158,255,.12) 0%, transparent 70%) }
/* 主題色改這裡的 rgba 第一個顏色 */

.hero h1 { font-family:var(--serif);
  font-size:clamp(2rem,4vw,3.2rem);
  background:linear-gradient(135deg,#fff 30%,var(--accent2));
  -webkit-background-clip:text; -webkit-text-fill-color:transparent }
/* 漸層結尾顏色可換為主題色，例：var(--teal) */

.stat-card::before { content:''; position:absolute; top:0; left:0; right:0;
  height:2px; background:var(--accent-bar, var(--accent)) }
/* 用 style="--accent-bar:var(--gold)" 覆蓋個別卡片顏色 */

.hero-stats { grid-template-columns:repeat(3,1fr) }
/* 若有 4 個改為 repeat(2,1fr) 或 repeat(4,1fr) */
```

---

## 8. Section 結構

每個 `<section>` 必須有 `id` 屬性供 scroll spy 使用。

```html
<section id="[錨點名稱]">
  <div class="section-header">
    <span class="section-num">01</span>
    <h2 class="section-title">[章節標題]</h2>
  </div>

  <p>[章節導語，1–2 句]</p>

  <!-- 各種元件... -->
</section>
```

```css
section { padding:4rem 0 1rem; border-bottom:1px solid var(--border) }
section:last-child { border-bottom:none }
.section-header { display:flex; align-items:center; gap:1rem; margin-bottom:2rem }
.section-num { font-size:.8rem; font-family:var(--mono);
  color:var(--accent); background:rgba(79,158,255,.1);
  border:1px solid rgba(79,158,255,.2);
  padding:.25rem .6rem; border-radius:4px; white-space:nowrap }
.section-title { font-family:var(--serif); font-size:1.9rem; color:var(--text) }
```

---

## 9. 元件庫

### 9-A. Info Block（資訊區塊）
```html
<!-- 藍色（預設）-->
<div class="info-block">
  <strong>[標題]</strong>
  [內文]
</div>

<!-- 顏色變體：加 class 即可 -->
<div class="info-block gold">...</div>    <!-- 金色 -->
<div class="info-block green">...</div>   <!-- 綠色 -->
<div class="info-block red">...</div>     <!-- 紅色 -->
```

```css
.info-block { background:var(--card); border-left:3px solid var(--accent);
  border-radius:0 var(--r) var(--r) 0; padding:1rem 1.4rem;
  margin:1.2rem 0; font-size:.9rem; color:var(--text2) }
.info-block.gold  { border-color:var(--gold)  }
.info-block.green { border-color:var(--green) }
.info-block.red   { border-color:var(--red)   }
.info-block strong { display:block; color:var(--text); margin-bottom:.3rem }
```

---

### 9-B. Research Card（文獻研究卡）
```html
<div class="research-grid">
  <div class="research-card">
    <div class="research-journal">期刊名 · 年份<sup class="cite">[n]</sup></div>
    <div class="research-title">
      <a href="[PubMed URL]" target="_blank">論文標題</a>
    </div>
    <div class="research-findings">
      <div class="research-finding">關鍵發現一</div>
      <div class="research-finding">關鍵發現二</div>
      <div class="research-finding">關鍵發現三</div>
    </div>
  </div>
</div>
```

```css
.research-card { background:var(--card); border:1px solid var(--border);
  border-radius:var(--r2); padding:1.4rem; transition:all var(--trans) }
.research-card:hover { border-color:var(--border2); transform:translateY(-1px) }
.research-journal { font-size:.72rem; font-family:var(--mono);
  color:var(--accent); opacity:.8; text-transform:uppercase; letter-spacing:.05em }
.research-title { font-size:.95rem; font-weight:600; color:var(--text); margin-bottom:.6rem }
.research-title a { color:inherit; border-bottom:1px solid rgba(232,241,252,.35) }
.research-finding { display:flex; gap:.6rem; font-size:.83rem; color:var(--text2) }
.research-finding::before { content:'→'; color:var(--accent); flex-shrink:0 }
```

---

### 9-C. Insight Grid（要點卡片組）
```html
<div class="insight-grid">
  <div class="insight-card">
    <div class="insight-n" style="background:rgba(79,158,255,.12);color:var(--accent)">
      A  <!-- 或數字 1 2 3 4 -->
    </div>
    <div class="insight-body">
      <h4>[標題]</h4>
      <p>[內文]</p>
    </div>
  </div>
</div>
```

```css
.insight-grid { display:grid; gap:1rem }
/* 雙欄：grid-template-columns:repeat(2,1fr) */
/* 單欄：預設 */
.insight-card { display:flex; gap:1.2rem; background:var(--card);
  border:1px solid var(--border); border-radius:var(--r2);
  padding:1.4rem 1.6rem; transition:all var(--trans) }
.insight-card:hover { border-color:var(--border2);
  transform:translateY(-1px); box-shadow:var(--shadow) }
.insight-n { width:2.5rem; height:2.5rem; min-width:2.5rem;
  border-radius:50%; display:grid; place-items:center;
  font-weight:700; font-size:1rem; font-family:var(--mono) }
/* 圓形顏色在 style="" 裡設定，保持彈性 */
```

---

### 9-D. Stage Grid（階段卡片）
```html
<div class="stage-grid">
  <div class="stage-card" style="--stage-color:var(--accent)">
    <div class="stage-badge">Stage 01</div>
    <div class="stage-name">[階段名稱]</div>
    <div class="stage-en">[英文名]</div>
    <ul>
      <li>[要點]</li>
    </ul>
    <div class="stage-tool">量測指標：<span>[指標]</span></div>
  </div>
</div>
```

```css
.stage-grid { display:grid; grid-template-columns:repeat(2,1fr);
  gap:1.2rem; margin:1.5rem 0 }
.stage-card { background:var(--card); border:1px solid var(--border);
  border-radius:var(--r2); padding:1.6rem; position:relative; overflow:hidden }
.stage-card::before { content:''; position:absolute; top:0; left:0; right:0;
  height:3px; background:var(--stage-color, var(--accent)) }
/* 用 style="--stage-color:var(--gold)" 覆蓋顏色 */
.stage-badge { font-size:.72rem; font-family:var(--mono);
  color:var(--stage-color, var(--accent));
  background:rgba(79,158,255,.1); border:1px solid currentColor;
  padding:.2rem .6rem; border-radius:4px; margin-bottom:.8rem }
```

---

### 9-E. Strategy Steps（條列步驟）
```html
<div class="strategy-steps">
  <div class="strategy-step">
    <div class="step-rank" style="background:rgba(79,158,255,.12);color:var(--accent)">1</div>
    <div class="step-content">
      <div class="step-title">[步驟標題]</div>
      <div class="step-desc">[說明]</div>
    </div>
  </div>
</div>
```

```css
.strategy-steps { display:flex; flex-direction:column; gap:.6rem; margin:1.2rem 0 }
.strategy-step { display:flex; align-items:flex-start; gap:1rem;
  background:var(--card); border:1px solid var(--border);
  border-radius:var(--r); padding:1rem 1.2rem }
.step-rank { width:2rem; height:2rem; min-width:2rem; border-radius:50%;
  display:grid; place-items:center; font-weight:700;
  font-size:.85rem; font-family:var(--mono); flex-shrink:0 }
/* 顏色在 style="" 設定 */
.step-title { font-weight:600; color:var(--text); font-size:.92rem; margin-bottom:.2rem }
.step-desc { font-size:.83rem; color:var(--text2) }
```

---

### 9-F. Big Stat Row（大數字統計列）
```html
<div class="big-stat-row">
  <div class="big-stat">
    <div class="num">[數字]</div>
    <div class="denom">[單位/分母]</div>
    <div class="desc">[說明]<sup class="cite">[n]</sup></div>
  </div>
</div>
```

```css
.big-stat-row { display:grid; grid-template-columns:repeat(3,1fr);
  gap:1rem; margin:1.5rem 0 }
/* 2欄：repeat(2,1fr)  4欄：repeat(4,1fr) */
.big-stat { background:var(--card); border:1px solid var(--border);
  border-radius:var(--r2); padding:1.5rem; text-align:center }
.big-stat .num { font-size:2.8rem; font-weight:700;
  font-family:var(--mono); color:var(--gold); line-height:1 }
/* 顏色：預設 gold，可加 class 或 style 改為 var(--accent) / var(--green) */
.big-stat .desc { font-size:.8rem; color:var(--text3); margin-top:.5rem }
```

---

### 9-G. Table（表格）
```html
<div class="table-wrap">
  <table>
    <thead>
      <tr>
        <th>欄一</th><th>欄二</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>[第一欄，自動加粗]</td>
        <td>[其他欄]</td>
      </tr>
    </tbody>
  </table>
</div>
```

```css
.table-wrap { overflow-x:auto; margin:1.5rem 0;
  border-radius:var(--r); border:1px solid var(--border) }
table { width:100%; border-collapse:collapse; font-size:.875rem }
thead th { background:var(--surface); font-size:.75rem;
  text-transform:uppercase; letter-spacing:.08em; color:var(--text3) }
tbody td:first-child { color:var(--text); font-weight:500 }
/* 特別強調欄位：color:var(--accent2) !important; font-weight:600 */
```

---

### 9-H. Timeline（時間軸）
```html
<div class="timeline">
  <div class="tl-item">
    <div class="tl-dot" style="background:rgba(79,158,255,.15);color:var(--accent)">01</div>
    <div class="tl-body">
      <div class="tl-title">[標題]</div>
      <div class="tl-desc">[說明]</div>
    </div>
  </div>
</div>
```

```css
.timeline { position:relative; margin:1.5rem 0 }
.timeline::before { content:''; position:absolute; left:1.4rem;
  top:0; bottom:0; width:2px; background:var(--border) }
.tl-dot { width:2.8rem; height:2.8rem; border-radius:50%;
  display:grid; place-items:center; font-weight:700;
  font-size:.8rem; font-family:var(--mono); position:relative; z-index:1 }
```

---

### 9-I. SVG 內嵌圖表
- 用於路徑示意、力向量比較、肌肉激活等視覺化
- 統一 viewBox 起點：`viewBox="0 0 [W] [H]"`，寬建議 320–500
- 背景 rect：`fill="#162238"`（即 --card）
- 文字 fill：`#a8bed8`（--text2）或 `#6b8aaa`（--text3）
- 線條顏色使用對應的 CSS 變數色碼直接寫入
- 加 `<div class="diagram-wrap">` 包裹，`max-width: 420px; width: 100%`

---

### 9-J. EMG Bar（肌肉激活條形圖）
```html
<div class="emg-bar-wrap">
  <div class="emg-row">
    <div class="emg-label">
      [肌肉名稱]
      <div class="emg-sublabel">[角色說明]</div>
    </div>
    <div class="emg-track">
      <div class="emg-fill" style="width:[xx]%;background:linear-gradient(90deg,[色1],[色2])">
        [描述標籤]
      </div>
    </div>
    <div class="emg-src"><sup class="cite">[n]</sup> 直接/推論</div>
  </div>
</div>
```

```css
.emg-row { display:flex; align-items:center; gap:.8rem; margin-bottom:.6rem }
.emg-label { font-size:.82rem; color:var(--text2); min-width:180px; flex-shrink:0 }
.emg-sublabel { font-size:.7rem; color:var(--text3) }
.emg-track { flex:1; background:var(--surface); border-radius:4px; height:18px; overflow:hidden }
.emg-fill { height:100%; border-radius:4px; font-size:.7rem;
  font-weight:700; color:rgba(8,16,30,.85);
  padding-left:.5rem; display:flex; align-items:center }
.emg-src { font-size:.7rem; color:var(--text3); min-width:56px; text-align:right }
```

**常用漸層配色（複製即用）：**
```
後鏈主動肌：linear-gradient(90deg,#2dd4bf,#34d399)
前鏈推力：  linear-gradient(90deg,#4f9eff,#7bc3ff)
穩定肌群：  linear-gradient(90deg,#f5b84b,#fdd888)
旋轉核心：  linear-gradient(90deg,#a78bfa,#c4b5fd)
警示肌群：  linear-gradient(90deg,#f87171,#fca5a5)
```

---

## 10. 參考文獻 REF 格式

### HTML 結構
```html
<section id="references">
  <div class="section-header">
    <span class="section-num">REF</span>
    <h2 class="section-title">參考文獻</h2>
  </div>
  <p>共 [n] 篇可驗證文獻，整合截至 [年月]。</p>

  <div class="ref-group">
    <div class="ref-group-title">一、[大類名稱]</div>

    <div class="ref-item">
      <div class="ref-num">1</div>           <!-- 藍色、等寬字體 -->
      <div class="ref-content">
        <div class="ref-title">             <!-- 白色、font-weight:500 -->
          <a href="[URL]" target="_blank">[論文標題]</a>
        </div>
        <div class="ref-meta">             <!-- 灰色 text3 -->
          作者縮寫 · 期刊 · 年份 · DOI/PMID · [一句話重點]
        </div>
      </div>
    </div>

  </div>
</section>
```

### CSS（必須嚴格遵守）
```css
.ref-group     { margin-bottom:2rem }
.ref-group-title { font-size:.75rem; text-transform:uppercase;
  letter-spacing:.12em; color:var(--text3);         /* 灰色 */
  margin-bottom:.8rem; padding-bottom:.5rem;
  border-bottom:1px solid var(--border) }
.ref-item      { display:flex; gap:1rem; padding:.8rem 0;
  border-bottom:1px dashed var(--border) }           /* 虛線分隔 */
.ref-item:last-child { border-bottom:none }
.ref-num       { font-family:var(--mono); font-size:.75rem;
  color:var(--accent);                               /* ← 藍色 */
  min-width:1.8rem; padding-top:.1rem }
.ref-content   { flex:1 }
.ref-title     { font-size:.88rem; color:var(--text);  /* ← 白色 */
  font-weight:500; margin-bottom:.2rem }
.ref-title a   { color:inherit;                        /* 繼承白色 */
  border-bottom:1px solid rgba(232,241,252,.35) }
.ref-title a:hover { border-color:var(--text) }
.ref-meta      { font-size:.78rem; color:var(--text3) } /* ← 灰色 */
```

### references.txt 格式（對應的文獻整理檔）
```
[主題] 運動科學 — 參考文獻總覽
====================================================
整合截至 [年月]

【一、[大類名稱]】

1. 作者, A., & 作者, B. (年份).
   論文標題.
   期刊名, 卷(期), 頁碼.
   DOI: xxx
   https://pubmed.ncbi.nlm.nih.gov/xxx/
   備註：[中文重點說明]

====================================================
共 [n] 篇文獻，均為可驗證的真實出版品
====================================================
```

---

## 11. Inline Citation 引用格式

```html
<!-- 正文中引用 -->
文獻資料<sup class="cite">[1]</sup>

<!-- 多篇引用 -->
研究結論<sup class="cite">[2]</sup><sup class="cite">[3]</sup>
```

```css
sup.cite {
  font-size: .72em;
  color: var(--accent);       /* 藍色 */
  font-weight: 700;
  margin-left: .15rem;
  white-space: nowrap;
  letter-spacing: -.01em;
}
```

**規則：**
- 引用緊貼在被引用文字後，數字前不加空格
- 編號與 REF 區塊一致（REF 無方括號，正文有方括號 `[n]`）

---

## 12. JavaScript

### Scroll Spy（Sidebar Active 狀態）
```javascript
const sections = document.querySelectorAll('section[id]');
const navLinks = document.querySelectorAll('nav a');
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      navLinks.forEach(a => a.classList.remove('active'));
      const active = document.querySelector(`nav a[href="#${entry.target.id}"]`);
      if (active) active.classList.add('active');
    }
  });
}, { rootMargin: '-20% 0px -70% 0px' });
sections.forEach(s => observer.observe(s));
```

### 漢堡選單（Mobile Sidebar）
```javascript
const sidebar = document.querySelector('aside');
const backdrop = document.getElementById('backdrop');
const hamTop = document.getElementById('ham-top');

function openSidebar()  { sidebar.classList.add('open');    backdrop.classList.add('open') }
function closeSidebar() { sidebar.classList.remove('open'); backdrop.classList.remove('open') }

hamTop.addEventListener('click', () =>
  sidebar.classList.contains('open') ? closeSidebar() : openSidebar()
);
backdrop.addEventListener('click', closeSidebar);
navLinks.forEach(a => a.addEventListener('click', () => {
  if (window.innerWidth <= 900) closeSidebar();
}));
```

---

## 13. Footer

```html
<footer>
  <div class="footer-inner">
    <p class="footer-note">
      本報告整合截至 [年月] 之學術研究成果，共 [n] 篇[同儕審查]文獻。
      [方法說明、免責聲明]。訓練建議應結合個人狀況並考慮尋求合格教練指導。
    </p>
    <div class="footer-badge">[主題 English] · [年月]</div>
  </div>
</footer>
```

```css
footer { background:var(--bg2); border-top:1px solid var(--border);
  padding:2.5rem 3rem; grid-column:1/-1 }  /* ← 跨越全寬 */
.footer-inner { max-width:900px; margin:0 auto;
  display:flex; gap:2rem; align-items:flex-start }
.footer-note { font-size:.82rem; color:var(--text3); line-height:1.7; flex:1 }
.footer-badge { background:var(--card); border:1px solid var(--border);
  border-radius:var(--r); padding:.7rem 1.2rem;
  font-size:.75rem; color:var(--text3); font-family:var(--mono) }
```

---

## 14. Responsive 規則

```css
@media (max-width: 900px) {
  .nav-links { display: none }
  #ham-top { display: flex }

  .page-wrap { grid-template-columns: 1fr; padding-top: 56px }

  /* Sidebar 變成滑入式 drawer */
  aside {
    position: fixed; top: 56px; left: -280px;
    height: calc(100vh - 56px); width: 260px; z-index: 100;
    transition: left .28s cubic-bezier(.4,0,.2,1);
    border-right: 1px solid var(--border2); box-shadow: var(--shadow2);
  }
  aside.open { left: 0 }

  main { padding: 0 1.5rem 4rem }
  .hero { padding: 2.5rem 1.5rem 2rem }
  .hero-stats { grid-template-columns: 1fr 1fr }
  .stage-grid  { grid-template-columns: 1fr }
  .insight-grid { grid-template-columns: 1fr }   /* 如原本雙欄 */
  .big-stat-row { grid-template-columns: 1fr }
  footer { padding: 2rem 1.5rem }
  .footer-inner { flex-direction: column }
}
```

---

## 15. 建立新報告 SOP

### Step 1：建立資料夾與 references.txt
```bash
mkdir -p /Users/jeff/Desktop/important/Website/IMM-eBook/ebk-[XXXX]-[YY]
mkdir -p /Users/jeff/Desktop/important/[topic]_science
```
- 先收集文獻，整理成 references.txt（格式見 Section 10）
- 確認所有文獻都有 PubMed 或 DOI 連結可驗證

### Step 2：建立 HTML 報告
基本 HTML 開頭模板（複製並填入即可）：

```html
<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>[主題]運動科學報告 · [English]</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Playfair+Display:ital,wght@0,700;1,700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
/* [貼上完整 CSS — 見各 Section 規格] */
</style>
</head>
<body>
<!-- Top Nav -->
<!-- Sidebar Backdrop -->
<div class="page-wrap">
  <aside><!-- Sidebar --></aside>
  <div>
    <main>
      <!-- Hero -->
      <!-- Section 00: 執行摘要 -->
      <!-- Section 01: ... -->
      <!-- ...依主題增減 Section... -->
      <!-- Section REF: 參考文獻 -->
    </main>
    <footer>...</footer>
  </div>
</div>
<script>/* JS：Scroll Spy + 漢堡選單 */</script>
</body>
</html>
```

### Step 3：更新 index.html
在 `.cat-grid` 的 `</div>` 前新增：
```html
<a href="./ebk-[XXXX]-[YY]/[topic]-report.html" class="cat-card" style="--cat-accent:var(--[color])">
  <div class="cat-num">[編號] · [英文名稱]</div>
  <div class="cat-category">[分類]</div>
  <div class="cat-title">[中文標題]</div>
  <p class="cat-desc">[描述，含文獻篇數]</p>
  <div class="cat-tags">
    <span class="cat-tag">[tag1]</span>
    <span class="cat-tag">[tag2]</span>
    <span class="cat-tag">[tag3]</span>
    <span class="cat-tag">[tag4]</span>
  </div>
  <div class="cat-cta">
    <span class="cat-status">科學報告 · 繁體中文</span>
    查看報告
  </div>
</a>
```

### Step 4：科學誠信檢查
- [ ] 所有文獻均可在 PubMed / DOI 獨立驗證
- [ ] 推論觀點已說明依據，非直接測量數據已標注
- [ ] 沒有使用無法找到的文獻（虛構 citation）
- [ ] REF 編號與正文 cite 編號一致

---

## 快速顏色查表

| 用途 | 顏色 | 變數 |
|------|------|------|
| 連結、ref-num、cite | 藍色 | `var(--accent)` |
| h3 標題、hover 文字 | 淺藍 | `var(--accent2)` |
| h4 標題、big-stat | 金色 | `var(--gold)` |
| 正向指標、成功 | 綠色 | `var(--green)` |
| 風險、警示 | 紅色 | `var(--red)` |
| 特殊分類 | 紫色 | `var(--purple)` |
| 主文字、ref-title | 近白 | `var(--text)` |
| 段落文字 | 灰藍 | `var(--text2)` |
| meta、label | 暗灰 | `var(--text3)` |
| 卡片背景 | 深藍 | `var(--card)` |
| Sidebar、footer | 次底 | `var(--bg2)` |

---

*本文件由 Claude Code 根據 IMM-eBook handstand 報告分析整理。*
*更新時機：有新元件加入、設計規格調整時同步更新本文件。*
