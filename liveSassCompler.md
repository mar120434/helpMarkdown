# Sass 使用方法

## Sass 的檔案格式

分為`.sass`和`.scss`，兩種。建議使用`.scss`檔，因為語法與`.css`類似。

## Sass 檔案放的位置

請新增一個`/sass`資料夾，用來放`.scss`檔案。

## VScode 的 Live Sass Compiler 套件設定

#### 排除下列資料夾中的 Sass 檔案轉成 CSS

```
"liveSassCompile.settings.excludeList": [
  "**/node_modules/**",
  ".vscode/**"
],
```

---

#### Sass 轉成 CSS 的檔案路徑

```
"liveSassCompile.settings.formats": [
  {
    "format": "expanded",
    "extensionName": ".css",
    "savePath": "/style"
  },
  {
      "format": "compressed",
      "extensionName": ".min.css",
      "savePath": "/dist"
  }
],
```

expanded 是沒有壓縮的 CSS 檔，通常路徑會設在`/style`<br>compressed 是壓縮的 CSS 檔，通常路徑會設在`/dist`

---

## Live Sass Compiler 使用方法

1.  用 VScode 開啟放在`/sass`的`.scss`
1.  點選下方 Watch Sass，旁邊有一個眼睛符號。<br>會在`/sass`的同一層產生`/style`和`/dist`資料夾，`/style`裡有轉譯好的 CSS 的檔案，`/dist`裡有壓縮好的 CSS 的檔案。
