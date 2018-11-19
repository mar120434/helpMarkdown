# Angular 專案啟動步驟

## 前言

### 安裝環境

必須先安裝完 node.js，要利用 node.js 的 yarn 作為套件管理工具。

### 安裝 Angular CLI

```cmd
npm install -g @angular/cli
```

我們需要 CLI 提供的 ng 指令。

### 建立起始專案

```cmd
ng new 專案名稱 --skip-install
```

`--skip-install`跳過 npm 套件管理的安裝，因為我們要使用 yarn 的套件管理。

### yarn 的套件管理

```cmd
cd 專案名稱
yarn
```

建好專案後，先進去專案資料夾裡。輸入 `yarn` 後，node.js 的套件會被安裝完成。

## 專案模組啟用

### app.module.ts

```TypeScript
'要在瀏覽器運行 Angular Application，必須加 BrowserModule。'
import { BrowserModule } from '@angular/platform-browser';

'要使用 @NgModule，必須加 NgModule。'
import { NgModule } from '@angular/core';

'要使用 *NgIf 或 *NgFor 時，必須加 CommonModule。'
import { CommonModule } from '@angular/common';

'建立 form 表單時，要加 FormsModule，這部分也包含了 NgModel 的'操作。
import { FormsModule } from '@angular/forms';

'建立響應式表單 reactive forms 前，要加 ReactiveFormsModule。'
import { ReactiveFormsModule } from '@angular/forms';

'使用路由功能，必須加 RouterModule。'
import { RouterModule } from '@angular/router';

'對 server 使用 Http Request 時，必須加 HttpClientModule。'
import { HttpClientModule } from '@angular/common/http';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    CommonModule,
    FormsModule,
    ReactiveFormsModule,
    RouterModule,
    HttpClientModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
