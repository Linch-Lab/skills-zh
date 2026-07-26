# HTML 報告格式

架構審查渲染為 OS 暫存目錄中的單一自包含 HTML 檔案。Tailwind 和 Mermaid 來自 CDN。

## 鷹架

```html
<!doctype html>
<html lang="zh">
  <head>
    <meta charset="utf-8" />
    <title>架構審查——{{repo 名稱}}</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script type="module">
      import mermaid from "https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.esm.min.mjs";
      mermaid.initialize({ startOnLoad: true, theme: "neutral", securityLevel: "loose" });
    </script>
  </head>
  <body class="bg-stone-50 text-slate-900 font-sans">
    <main class="max-w-5xl mx-auto px-6 py-12 space-y-12">
      <header>...</header>
      <section id="candidates" class="space-y-10">...</section>
      <section id="top-recommendation">...</section>
    </main>
  </body>
</html>
```

## 候選者卡片

每張卡片：標題、標記列、檔案、前後圖表、問題、解決方案、效益、ADR 標註（若適用）。

## 圖表模式

混合 Mermaid 和手工 CSS/SVG。模式：Mermaid 圖、手工框線箭頭、橫截面、質量圖、呼叫圖坍縮。

## 頂部推薦區段

一張較大卡片。候選者名稱、一行原因。

## 語調

簡潔英文，使用 `/codebase-design` skill 的架構名詞和動詞。
