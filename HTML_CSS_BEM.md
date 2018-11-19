# HTML, CSS 的 BEM 命名規則

## 前言

> 利用 BEM 的命名方法來讓 HTML 跟 CSS 文件更容易撰寫和維護。

- **Block**：區塊命名
- **Element**：物件命名，使用雙底線 **( \_\_ )**
- **Modifier**：修飾符命名，使用雙破折號 **( -- )**

## 使用規則與範例

### Block

> 由 拉丁字母、數字、底線、破折號 組成，代表 **Block** 的名稱：

```HTML
<!-- HTML -->

<div class="block">...</div>
<div class="block-area">...</div>
<div class="block_area">...</div>
<div class="blockArea">...</div>
```

```CSS
/* CSS
只使用 class
不使用 tag、id
Block 不會放在其他 Block 或 Element 之下 */

.block { /*CSS屬性*/ }
.block-area { /*CSS屬性*/ }
.block_area { /*CSS屬性*/ }
.blockArea { /*CSS屬性*/ }
```

### Element

> 由 Block 名稱加上 雙底線 **( \_\_ )** 再加上 一個拉丁字母或數字，代表 **Element** 的名稱：

```HTML
<!-- HTML -->

<div class="block">
  <span class="block__element"></span>
  <!-- block__element 是 Element 的名稱
  Element 要放在 Block 之下 -->
</div>
```

```CSS
/* CSS
只使用 class
不使用 tag、id
Element 不會放在其他 Block 或 Element 之下 */

/* 好的寫法 */
.block__element { /*CSS屬性*/ }

/* 非常爛的寫法 */
.block .block__element { /*CSS屬性*/ }
```

### Modifier

> 用來標記 Block 或 Element 的行為或狀態，先加上雙破折號 **( -- )** 再加上 一組拉丁字母或數字：

```HTML
<!-- HTML -->
<!-- 好的寫法 -->
<div class="block block--modifier">
  <!-- block--modifier 是 Modifier 的名稱，Modifier 要放在 Block 或 Element 後面 -->
  <div class="block__element block__element--modifier-big">
    <!-- block__element--modifier-big 是 Modifier 的名稱 -->
  </div>
</div>

<!-- 非常爛的寫法 -->
<div class="block--modifier">...</div>
```

```CSS
/* CSS
/* 只使用 class */
.block--modifier { /*CSS屬性*/ }

/* Modifier 後面加上 Element */
.block--modifier .block--element { /*CSS屬性*/ }

/* Element Modifier */
.block__element--modifier-big { /*CSS屬性*/ }
```
