# TypeScript 學習筆記 (拉拉主講的內容整理)

### TypeScript：提供強型別語法的 JavaScript 超集合

## TypeScript 優點

- 可以在編譯時期做型別檢查
- 透過 **預編譯器**，相容 JavaScript 語法
- 與 ES6 結合，並提供更多支援寫法
- 有 Microsoft 跟 Google 支援

## 基本型別

所有 **變數** 可以在宣告的時候指定型別。如果被指派不符的型別內容，編譯器會出現錯誤提示。

### JavaScript 型別

- boolean
- number
- string
- array

### TypeScript 增加型別

- enum (列舉)
- any (任意型別)
- void (無型別)

### Number

- TypeScript 所有數值都是浮點數

### String

- 使用雙引號 **( " )** 或單引號 **( ' )** 表示字串
- 可使用 ES6 **字串樣板 (Template Literals)**
  - **( ` )** 重音符號表示字串樣板
  - 字串樣板中可以使用 **${variable}** 放置變數
  - 範例：
    ```TypeScript
    var name: string = `Gene`;
    var age: number = 37;
    var sentence: string = `Hello, my name is ${ name }. I'll be ${ age + 1 } years old next month.`;
    ```

### Array

- 定義陣列的方式有兩種
  - 在宣告型別後面加上中括號 **( [ ] )**，宣告為某型別的 Array
    ```TypeScript
    var idList: number[] = [1, 2, 3];
    ```
  - 使用陣列泛型，Array<元素型別>
    ```TypeScript
    var list: Array<number> = [1, 2, 3];
    ```

### Enum 列舉型別

enum 是針對 JavaScript 標準資料型別的擴充。可以使用 enum 為一組數值賦予有意義的名稱。

```TypeScript
enum Color {Red, Green, Blue};
var c: Color = Color.Green;
```

- 預設情況下，從 0 開始為元素編號。

  ```TypeScript
  enum Color {Red, Green, Blue};
  // Color.Red = 0, Color.Green = 1, Color.Blue = 2
  ```

- 可以指定成員的數值
  ```TypeScript
  enum Color {Red = 1, Green, Blue};
  // Color.Red = 1, Color.Green = 2, Color.Blue = 3
  ```
- 全部用手動賦值
  ```TypeScript
  enum Color {Red = 1, Green = 2, Blue = 4};
  // Color.Red = 1, Color.Green = 2, Color.Blue = 4
  ```

### Any 任意型別

當我們不希望 TypeScript 的 型別檢查器 對 來自第三方的函式庫的變數 進行型別檢查，可以利用 any 來標記這些變數。

```TypeScript
var notSure: any = 4;
notSure = "maybe a string instead";
notSure = false;
// 因為型別為 any ，所以不會報錯
```

為了程式穩定還是避免使用 any，只建議在快速開發時使用。

### Void

通常使用在函式沒有回傳值時。

```TypeScript
function showAlert: void {
  alert("this is the message");
}
```

## 基本語法

### 變數

- var (function scope)
- let (block scope)
- const (常數)

### 使用 let 取代 var

let 不會有 hoisting。hoisting 指的是 變數宣告會被隱含提升到其所在的區域內的頂端。

- **var**<br>
  在執行時期，所有 var 變數都會自動被 hoisting 。如果程式中有參考到 var 定義的變數時，會變成 undefined，而不會產生 ReferenceError。因此容易在 Debug 時造成誤解。
- **let**
  - let 變數不會被 hoisting ，它是依附在大括號 **( { } )** 的區域中。當程式中有參考 let 變數時，因為作用區塊不同會產生 ReferenceError。此行為較符合常用的其他程式語言。
  - let 變數在相同 scope 時，不會被重新宣告。
    - for loop 中使用 var。由於非同步的關係，迴圈會先跑完，var 變數的值會 **重新定義(redeclared)**。所以以下 callback function 的 i 值，永遠會是最後一個。
      ```TypeScript
      var names = ['a', 'b', 'c'];
      function asyncFn(names){
        for (var i in names) {
          $.get("/users/" + names[i], function(){
            console.log("ajax get for", names[i]);
          });
          //三次結果都是 "ajax get for c"
        }
      }
      ```
    - for loop 改用 let。雖然迴圈先跑完，但 let 變數不會 **redeclared**，所以 callback function 的 i 值，會依附著 for loop 做變化
      ```TypeScript
      let names = ['a', 'b', 'c'];
      function asyncFc(names){
        for (let i in names){
          $.get("/users/" + names[i], function(){
            console.log("ajax get for", names[i]);
          });
        }
        // 結果分別為
        // "ajax get for a"
        // "ajax get for b"
        // "ajax get for c"
      }
      ```
- **const**<br>

  > 使用 const 定義常數的值，會讓程式碼更容易被理解。<br>
  > const 不會有 hoisting 的問題。

  通常在寫判斷式時會使用沒有定義的數值(又稱為 Magic Number)，這是不好的寫法，因為不清楚數值代表什麼，例如：

  ```TypeScript
  function colors(colorNumber) {
    if(colorNumber.length > 3){ // 沒有人會知道這裡的 3 是指什麼
      // ...
    }
  }
  ```

  改用 const 讓程式碼更容易閱讀，例如：

  ```TypeScript
    function colors(colorNumber) {
      const MAX_COLOR_NUMBERS = 3; // 使用 const 來讓數值有定義
      if(colorNumber.length > MAX_COLOR_NUMBERS){
        // ...
      }
    }
  ```

  const 是唯讀的，一旦定義了就無法改變，例如：

  ```TypeScript
    function colors(colorNumber) {
      const MAX_COLOR_NUMBERS = 3;
      MAX_COLOR_NUMBERS = 4;
      // 無法改變 MAX_COLOR_NUMBERS 的數值，MAX_COLOR_NUMBERS 還是等於 3
      if(colorNumber.length > MAX_COLOR_NUMBERS){
        // ...
      }
    }
  ```

  宣告 const 的時候，一定要指定初值，否則會產生 SyntaxError，例如：

  ```TypeScript
    function colors(colorNumber) {
      const MAX_COLOR_NUMBERS;
      // 沒有宣告初值，產生 SyntaxError
      MAX_COLOR_NUMBERS = 3;
      if(colorNumber.length > MAX_COLOR_NUMBERS){
        // ...
      }
    }
  ```

### Function

> TypeScript 與 JavaScript function 不同的地方是，TypeScript 需要指定 **傳入參數** 和 **回傳值** 的 **型別**，避免傳入不正確的參數而導致異常。

```TypeScript
function goodFunction(goods: number): number {
  return goods * 2;
  // 第一個 (小括弧裡) 的 number 指的是傳入參數的型別
  // 第二個 number 指的是回傳值的型別
}
```

- 選擇性參數與預設參數<br>

  > 在 TypeScript 中，函式呼叫時傳遞的參數個數必須與函式期望的一致，否則編譯器會報錯，例如：

  ```TypeScript
  function fullName(firstName: string, lastName: string) {
    return firstName + " " + lastName;
  }
  var result1 = fullName("NIKE", "adidas");
  // 滿足兩個參數的條件，正確
  var result2 = fullName("NIKE");
  // 少了一個參數，回報錯誤
  var result3 = fullName("NIKE", "adidas", "PUMA");
  // 多了一個參數，回報錯誤
  ```

  > 在 JavaScript function 的參數都是選擇性的。當沒有傳值時，參數值會是 undefined。<br>TypeScript 要實作 **選擇性參數** 的功能，必須在參數名稱後使用問號 **(" ? ")**，標示此參數為 **選擇性 (optional)**。把剛才的例子修改：

  ```TypeScript
  function fullName(firstName: string, lastName?: string) {
    if(lastName){
      return firstName + " " + lastName;
    }else{
      return firstName;
    }
  }
  var result1 = fullName("NIKE", "adidas");
  // 滿足兩個參數的條件，正確
  var result2 = fullName("NIKE");
  // lastName 為選擇性參數，就算沒有仍回傳正確
  var result3 = fullName("NIKE", "adidas", "PUMA");
  // 多了一個參數，回報錯誤
  ```

  > 也可以在參數後面指定 **預設值**。當沒有傳入參數時，可以根據預設值來處理。

  ```TypeScript
  function fullName(firstName: string, lastName: string = '') {
    if(lastName !== ''){
      return firstName + " " + lastName;
    }else{
      return firstName;
    }
  }
  var result1 = fullName("NIKE", "adidas");
  // 滿足兩個參數的條件，正確
  var result2 = fullName("NIKE");
  // lastName 有預設值，就算沒有仍回報正確
  var result3 = fullName("NIKE", "adidas", "PUMA");
  // 多了一個參數，回報錯誤
  ```

  > - 選擇性參數 `lastName?: string`
  > - 參數預設值 `lastName: string = ''`
  >
  > 以上兩者都必須放在 **function 的尾端**，否則會報錯。

- 其餘參數
  > JavaScript ES6 提供了 **其餘參數 (Rest parameters)**。可以把 **多個參數** 同時傳入一個函式，其餘參數會把傳入的參數轉換成一個 **數值陣列**。
  ```JavaScript
  function sum(...numbers) { // ...number 為其餘參數，參數前面加 ( ... )
    let result = 0;
    numbers.forEach(function(number) {
      result += number;
    });
    return result;
  }
  console.log(sum(1)); // 1
  console.log(sum(1, 2, 3, 4, 5)); // 15，傳入的參數被轉為數值陣列
  ```
  > TypeScript 中使用其餘參數，必須指定參數型別為 array。
  ```TypeScript
  function fullName(firstName: string, ...restOfName: string[]){
    // restOfName 為其餘參數，後方有指定型別 string[]
    return firstName + ' ' + restOfName.join(' ');
  }
  var employeeName = fullName('NIKE', 'adidas', 'PUMA');
  ```
- **arrow function**

  > 寫法如下：

  ```JavaScript
  fnName(x, y) => {
    // ...
  }
  ```

  > 傳統寫法與 ES6 比較：

  ```JavaScript
  // ES5
  function foo(x, y) {
    x += 1;
    y += 1;
    return x + y;
  }
  // ES6
  foo(x, y) => {
    x += 1;
    y += 1;
    return x + y;
  }
  ```

  > **arrow function** 解決了 **this** 指向的問題。

  ```JavaScript
  // 沒有使用 arrow function 的情況
  class Animal {
    constructor() {
      this.type = 'animal';
    }
    says(say) {
      setTimeout(function() {
        console.log(this.type + ' says' + say);
      }, 1000)
    }
  }
  var animal = new Animal();
  animal.says('hi'); // 結果為 undefined says hi
  ```

  > 上面的例子中，由於 `setTimeout(function(){})` 影響的關係，this 指向了 window.this，而產生 undefined，因為 window.this.type 沒有定義。

  > 改用 **array function** 修改上面的程式碼：

  ```JavaScript
  // 使用 arrow function
  class Animal {
    constructor() {
      this.type = 'animal';
    }
    says(say){
      setTimeout( () => {
        console.log(this.type + ' says ' + say);
      }, 1000);
    }
  }
  var animal = new Animal();
  animal.says('hi'); // animal says hi，解決了 this 的指向問題
  ```

  > 使用 () => 之後，函數內的 this 就是 **定義時所在的對象**，而不是使用時所在的對象。

  > () => 沒有自己的 this，它的 this 是繼承外面的，因此內部的 this 就是外層代碼區域的 this。

- **for...of** 和 **for...in**

  - **for...of 是遍歷屬性值 (property values)**
  - **for...in 是遍歷屬性名 (property names)**
    > 範例

  ```JavaScript
  let myobj = {a: 1, b: 2};
  myobj[Symbol.iterator] = function() {
    let properties = Object.keys(this);
    let count = 0;
    let isDone = false;
    return {
      next: () => {
        if(count >= properties.length) {
          isDone = true;
        }
        return {
          done: isDone,
          value: this[properties[count++]]
        };
      } // end next
    }; //end return
  }

  console.log('for...of');
  for(let val of myobj) {
    console.log('property value', val);
  }
  console.log('for...in');
  for(let prop in myobj) {
    console.log('property name', prop);
  }
  ```

### Class

- 基本語法

  > 寫法跟 ES6 相同，會有屬性、建構式的特性，範例：

  ```TypeScript
  class Student {
    // 屬性
    name: string;

    // 建構式
    constructor(name: string) {
      this.name = name;
    }

    // function 寫法，就像這樣子
    getPhoneNumber(): string {
      // 程式邏輯
    }
  }
  ```

- **property 屬性**

  > 屬性預設都是 **public**。不想要被外部存取時，要加上 **private**。

  ```TypeScript
  class Student {
    name: string;
    age: number;
    private birth: Date;
  }

  var student = new Student();

  console.log(student.name); // 正常
  console.log(student.birth); // 錯誤顯示
  ```

  > 可以把屬性宣告為 static。在使用時，不需要實體化就能呼叫。

  ```TypeScript
  class Student {
    static type: string;
  }

  Student.type;
  ```

  > 可以使用 **等號 ( = )** 直接指定屬性預設值。

  ```TypeScript
  class Student {
    name: string = 'Student';
  }
  ```

- **constructor 建構式**

  > 很像在寫 Java

  ```TypeScript
  class Student {
    name: string;
    age: number;

    constructor(name: string, age: number) {
      this.name = name;
      this.age = age;
    }
  }

  var student = new Student('Kirk', 18);
  console.log(`Student name: ${student.name}, age: ${student.age}`);
  ```

  > TypeScript 提供的建構式速寫語法。

  ```TypeScript
  class Student {
    constructor(public name: string, public age: name) {}
  }

  var student = new Student('Kirk', 18);
  console.log(`Student name: ${student.name}, age: ${student.age}`);
  ```

### Interface

- **interface basic**

  > 跟 Java 寫法相似，**interface (介面)** 是一種規格

  ```TypeScript
  interface IClock{
    currentTime: Date; // 屬性
    setTime(d: Date); // 方法
  }
  ```

  > 當一個類別實踐一個 interface，表示它必須實踐這個規格。<br>
  > 實作類別的關鍵字是 **implements**

  ```TypeScript
  class Clock implements IClock {
    currentTime: Date;
    constructor(public h:number, public m: number) {
      setTime(d: Date) {
        this.currentTime = d;
      }
    }
  }

  var myClock: IClock = new Clock(12, 60);
  myClock.setTime(new Date());
  ```

- **hybrid types**

  > 另一種用法：TypeScript 可以利用 interface 來達成 **複雜型別 (complex type)**

  ```TypeScript
  interface IPerson{
    name: string;
    age: number;
  }
  function showPerson(person: IPerson) {
    console.log('name: ', person.name, ' age: ', person.age);
  }
  ```

- **interface 擴充**

  > 就像 class 一樣，interface 也能彼此擴充。<br>
  > 可以快速複製一個 interface 的成員到另一個 interface，這樣更有利於 interface 的重用性，讓程式更彈性。

  ```TypeScript
  interface  Shape {
    color: string;
  }

  interface Square extends Shape { // interface 的擴充
    sideLength: number;
  }

  // 這個例子把 interface 當作複雜型別
  // <型別> 是泛用的用法，之後會再提到
  let square = <Square>{};
  square.color = 'blue';
  square.sideLength = 10;
  ```

### Generic

> 當我們在建構 Function、Interface 及 Class 時，會希望這些 Component 能夠被重複運用。**泛型 (Generic)** 提供了一個彈性的寫法。

<center>語法：&lt;T&gt;</center>

> 在撰寫 Function、Interface 及 Class 時，不指定變數的 **具體型別**，而是用 **型別參數** 來代替。當未來在使用這個 Component 時，這個 **型別參數** 會由一個具體的型別所代替。_泛用多種型別_。

- **Generic Functions**

  ```TypeScript
  function LogAndReturn<T>(thing: T): T {
    // 一個泛型 function (第一個 <T>)
    // 傳入參數 (第二個 T) 跟 回傳參數 (第三個 T) 也都是泛型
    console.log(thing);
    return thing;
  }
  let stringLog: string = LogAndReturn<string>('string log');
  let numberLog: number = LogAndReturn<number>(1000);

  let newBook: Book = {title: 'This is a book.'};
  let vookLog: Book = LogAndReturn<Book>(newBook);
  ```

- **Generic Interfaces**

  ```TypeScript
  interface Inventory<T> {
    addItem: (newItem: T) => void;
    getAllItems: () => Array<T>;
  }

  let bookInventory: Inventory<Book>;
  let allBooks: Array<Book> = bookInventory.getAllItems();
  let comicInventory: Inventory<Comic>;
  let allComics: Array<Comic> = comicInventory.getAllItems();
  ```

- **Generic Classes**

  ```TypeScript
  class Shelf<T> {
    private _items: Array<T> = new Array<T>();
    add(item: T): void {
      this._items.push(item);
    }
  }

  let newBook: Book = {title: 'CSS Secrets'};
  let bookShelf: Shelf<Book> = new Shelf<Book>();
  bookShelf.add(newBook);

  let newNumber: number = 1000;
  let numberShelf: Shelf<number> = new Shelf<number>();
  numberShelf.add(newNumber);
  ```

- **Generic Constraints**

  > 有時候商業邏輯需要操作宣告為泛型的類別成員。

  ```TypeScript
  class Shelf<T> {
    private _items: Array<T> = new Array<T>();

    add(item: T): void {
      this._items.push(item);
    }

    printTitles(): void {
      this._items.forEach(item => console.log(item.title));
      // compile 會告訴你 T 沒有 title 這個屬性
    }
  }
  ```

  > 遇到這種狀況，需要幫 Generic 加上限制。

  ```TypeScript
  interface ShelfItem {
    title: string;
  }

  class Shelf<T extends ShelfItem> { // 增加的限制
    private _items: Array<T> = new Array<T>();

    add(item: T): void {
      this._items.push(item);
      // 因為增加限制，而恢復正常
    }

    printTitles(): void {
      this._items.forEach(item => console.log(item.title));
    }
  }
  ```

  ```TypeScript
  let newBook: Book = {title: 'CSS Secrets'};
  let bookShelf: Shelf<Book> = new Shelf<Book>();
  bookShelf.add(newBook);
  bookShelf.printTitles(); // Correct，因為 book 有 title 屬性

  let newNumber: number = 1000;
  let numberShelf: Shelf<number> = new Shelf<number>();
  numberShelf.add(newNumber);
  numberShelf.printTitle(); // Error，因為 number 沒有 title 屬性
  ```

### Modules

> 在 TypeScript 中 **組織程式碼的方法**
>
> - 外部模組 - module<br>
>   個別模組的功能不同，利用 import / export 來互相引用彼此公開的功能。
> - 內部模組 - namespace<br>
>   個別模組的功能相近，使用 namespace 集中功能。

- **module** 外部模組
  - 使用 **export** (用法同 ES6)<br>
    將想要分享的變數、含式、類別及介面做 **公開**
  - 使用 **import** (用法同 ES6)<br>
    **引用** 不同檔案並設定公開的變數、含式、類別及介面
  ```TypeScript
  // MyExport.ts
  export class SomeType { /* ... */ }
  export function someFn { /* ... */ }
  ```
  ```TypeScript
  // App.ts
  import { SomeType, someFn } from './MyExport';
  let x = new SomeType();
  let y = someFn();
  ```
- **namespace** 內部模組 (命名空間)

  - namespace 補充。請先觀察以下寫法有什麼缺點：

    ```TypeScript
    interface Shape {
      area(h: number, w: number): number;
    }

    class Square implements Shape {
      area(h: number, w: number) {
        return h * w;
      }
    }

    class Triangle implements Shape {
      area(h: number, w: number) {
        return (h * w) / 2;
      }
    }

    let s = new Square();
    console.log(s.area(10, 5)); // 50
    let t = new Triangle();
    ```

    > Shape, Square, Triangle 皆放在 global namespace。放在 global namespace 的缺點是容易造成名稱衝突。

    > 寫法可修改成模組化，關鍵字使用 namespace。

    ```TypeScript
    namespace Geometric {
      const HALF = 0.5;

      export interface Shape {
        area(h: number, w: number): number;
      }

      export class Square implements Shape {
        area(h: number, w: number) {
          return h * w;
        }
      }

      export class Triangle implements Shape {
        area(h: number, w: number) {
          return (h * w) * HALF;
        }
      }
    } // 所以 global namespace，只有 Geometric 這個物件
    ```

    > 我們希望 interface 跟 class 是公開的，所以使用 export。而變數 HALF 是實現的細節，就不必使用 export，因此變數 HALF 在模組外是不可見的。

    ```TypeScript
    // test.ts
    namespace Geometric{
      const HALF = 0.5;

      export interface Shape {
        area(h: number, w: number): number;
      }

      export class Square implements Shape {
        area(h: number, w: number) {
          return h * w;
        }
      }
    }
    ```

    ```JavaScript
    // test.js
    var Geometric;
    (function (Geometric) {
      var HALF = 0.5;
      var Square = (function () {
        function Square() {
        }
        Square.prototype.area = function (h, w) { return h * w };
        return Square;
      }());
      Geometric.Square = Square;
    })(Geometric || (Geometric = {}));
    ```

    > 可以觀察到編譯成 ES3 之後，模組被包裝成立即函式，因此避免了全域環境汙染。

    將模組拆分成多個檔案

    > 隨著應用的擴展，我們希望將程式拆分成多個文件。使每個檔案的功能更單純，更方便維護。(單一職責原則)

    ```TypeScript
    // Shape.ts
    namespace Geometric {
      export interface Shape {
        area(h: number, w: number): number;
      }
    }
    ```

    ```TypeScript
    // Square.ts
    /// <reference path="Shape.ts"></reference>
    namespace Geometric {
      export class Square implements Shape {
        area(h: number, w: number) {
          return h * w;
        }
      }
    }
    ```

    ```TypeScript
    // Triangle.ts
    /// <reference path="Shape.ts"></reference>
    namespace Geometric {
      export class Triangle implements Shape {
        area(h: number, w: number) {
          return (h * w) / 2;
        }
      }
    }
    ```

    > 雖然被拆分成多個單獨的檔案，但是它們都在為同一個模組貢獻功能。檔案中有參考的地方，使用 reference 來做引用。

    ```TypeScript
    // App.ts
    /// <reference path="Shape.ts"></reference>
    /// <reference path="Square.ts"></reference>
    /// <reference path="Triangle.ts"></reference>

    let s = new Geometric.Square();
    console.log(s.area(10, 5)); // 50
    let t = new Geometric.Triangle();
    console.log(t.area(10, 5)); // 25
    ```

    - 別名

      > 當取用模組的 path 比較長時，可以使用 `import q = x.y.z` 的語法，給常用的模組取一個簡短的名稱

      ```TypeScript
      namespace Shapes {
        export namespace Polygons {
          export class Square{}
          export class Triangle{}
        }
      }

      // 沒有使用別名前
      var test1 = new Shapes.Polygons.Square();
      var test2 = new Shapes.Polygons.Triangle();

      //使用別名之後
      import pg = Shapes.Polygons;
      var sq = new pg.Square();
      var tri = new pg.Triangle();
      ```

### Type Script Compiler (tsc)

> - 瀏覽器不會直接執行 TypeScript，tsc 會將 TypeScript 轉譯成 JavaScript。
> - 所有轉譯的設定，被定義在 tsconfig.json 這個檔案中。

- **tsconfig.json**<br>
  基本設定
  ```json
  {
    "compilerOptions": {
      // 如何轉譯的設定
      "target": "es5",
      // 轉換成的 JavaScript 的版本，可以轉換成 ES3, ES5, ES6
      "module": "commonjs",
      // 哪一種模組化的寫法像是 AMD 的 RequireJS 還有像是 CommonJS 還有 ES6 import, export 寫法
      "typeRoots": ["../../node_modules/@types"]
      // 指定 TypeScript 的定義檔放在哪裡
    },
    "compileOnSave": true,
    "exclude": ["node_modules/＊"]
    // 排除那些不要去編譯，通常 node_modules 裡的都已經編譯過了
  }
  ```

### Type Definition File 定義檔

- 當操作第三方函式庫 (Lib) 時，都需要定義檔 (\*.d.ts)
- 定義檔用來揭露第三方函式庫的型別資訊
- 不包含實作的細節，可以想像它是類似 c / c++ 的表頭檔 (\*.h)
- 有了 Lib 定義檔，就可在開發時期操作 Lib 做一些型別檢查

### Ambient Declaration

> **<center>ambient declaration = 周圍環境宣告</center>**
> 用來宣告一個在目前環境中已經有的變數。例如：已經載入 jQuery 了，但 TypeScript 不知道，透過這個設定可以讓 TypeScript 知道 $ 代表某個型別。

例子：
**<center>jquery.d.ts</center>**

```TypeScript
declare var jQuery: JQueryStatic;
declare var $: JQueryStatic;
```

**<center>knockout.d.ts</center>**

```TypeScript
declare var ko: KnockoutStatic;
```

- **Type Definition Sources**
  > 定義檔來源：
  > | Definitely Typed | Typings Type Definition<br>Manager (multiple sources) |
  > | ---------------- | ----------------------------------------------------- |
  > | **npm packages** | **Write your own!** |
  - Definitely Typed
    - Github 儲存庫，存放許多定義檔，可手動下載至專案內。
    - 舊的做法，目前不建議使用，除非真的找不到。
  - tsd
    - NPM 套件，用來搜尋跟下載 TypeScript 定義檔。
    - 所有下載的設定，可以在 tsd.json 看到。
    - 舊的做法，目前不建議使用。
  - typings
    - NPM 套件，用來搜尋跟下載 TypeScript 定義檔。
    - 可以指定多個下載來源
      - npm/github/bower/dt(DefinitelyTyped)
    - 所有下載的設定，可以在 typings.json 看到。
    - 舊的做法，目前不建議使用。
  - **@types**
    - 在 TypeScript 2.0 之後，盡量不使用 tsd 及 typings
    - 不須再額外安裝其它下載定義檔的套件，直接使用 npm<br>
      `npm install --save @types/lodash`
    - 所有下載的設定，可以在 package.json 看到。
    - 只需要在 tsconfig.json 指定 typeRoots，tsc 就知道去哪裡找到定義檔
      ```json
      "typeRoots": [
        "../node_modules/@types"
      ]
      ```

## 小結

- JavaScript 慢慢變成類似 Assembly Language (組合語言) 的地位
- 會用 ES2015 或是 TypeScript 的高階語法來開發 JavaScript
- 透過 **預編譯** 把高階語法轉譯成瀏覽器認識的 ES3 或 ES5
- TypeScript 的型別檢查、模組化及 OOP 特性，增加程式開發品質
