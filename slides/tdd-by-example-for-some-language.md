class: center, middle

<style>
.column-left{
  float: left;
  width: 40%;
  text-align: left;
}
.column-right{
  float: right;
  width: 55%;
  text-align: left;
}
</style>

# テスト駆動開発入門で<br />プログラミング言語を学ぶ

2019/10/18 @msfukui

---
class: center, middle

# 今回のテーマ

##「ちいさな成功体験」

![小さな恋のメロディ](slides/melody.jpg "Melody")

---
class: center, middle

##「ちいさな成功体験」<br />≒<br />「学びによる達成感」

---

# 三行で

* プログラミング言語を学ぶ際に

* 題材選びで困ったら

* 「テスト駆動開発入門」よいと思います

### 話さないこと

* なぜ、プログラミング言語を、学ぶ?

* プログラミング言語、何を、学ぶ?

### これはこれで話したい・お聞きしたい..

---

# どうやって学ぶ?

* `Hello World`

* 環境構築

* チュートリアル

* ドキュメントを読む

* 詳しい人のブログを読む

* サンプルアプリケーションを作る

* やったことをブログにまとめる

---

# 言語、どうやって学ぶ?

* `Hello World`

* 環境構築

* チュートリアル

* <b>ドキュメントを読む</b>

* 詳しい人のブログを読む

* サンプルアプリケーションを作る

* やったことをブログにまとめる

### 作る前にもうちょっと手を動かしたくなる

---

# テスト駆動開発入門

<div class="column-left">
  <img src="slides/tdd_by_example.jpg" width="360">
</div>
<div class="column-left">
  <img src="slides/tdd_by_example_jp_new.jpg" width="320">
</div>

---

# テスト駆動開発入門

* TDD の古典的な解説書

* テーマに沿って章毎に少しずつ実装を進める

* 第一部（多国通貨）: Java : 多国通貨とのそのレート変換のモデルを TDD で作る

* 第二部（xUnit）: Python : xUnit を TDD で作る

### 第一部だけに絞ってもよいと思います

---

# いいところ

* 章の区切りがいい感じ<br />
1章が5〜30分くらいで終わるボリューム。少しずつ進められる

* 言語固有のユニットテストフレームワークを学べる<br />
というか、調べないと進められないので、頑張って調べることになる

* 本なので終わりがある（ = 達成感が持てる）

# だめなところ

* クラス設計ができる言語じゃないとだめ<br />
アセンブラ言語、ニーモニック、C言語、 bash とかはきっと厳しい..<br />
関数型言語も微妙な気がする..

---

# 準備はこんな感じで

* github.com にレポジトリを作る<br />
tdd-by-example-for-typescript とか

* 通知用の slack channel を作る

* CI と カバレッジの設定を準備<br />
TravisCI, CodeClimate, etc..<br />
Webhooks とか GitHub Apps とか<br />

---

# README を書いて

* 環境の作り方<br />
どんな環境、テストフレームワーク、エディタで書くか決める

* テストの実行方法<br />
テストフレームワークの使い方を調べて書く<br />
テストコードのサンプルを一緒に書いておくとよい

* ディレクトリ構成の説明<br />
どこに何を入れるかの配置ルールを決める

* 参考文献へのリンク<br />
参考にしたブログ、公式サイトのリンクを書いておく

---

# 進める

* 章毎にブランチを作って

* コード書いたら push して

* master ヘの pull request を切る

* 終わったら merge ボタン押してマージ

* 進める中で詰まったら pull request の中に書く<br />
解決しなかったら Issue に切り出して先に進める<br />
解決したらその方法などもメモる

### つまり、普通に開発するのと同じように、進める

---

# 気をつけたところ

<div class="column-left">
  <a href="https://github.com/msfukui/tdd-by-example-for-typescript/issues?q=is%3Aissue+is%3Aclosed" target="_blank"><img src="slides/issues.png" width="460"></a>
</div>
<div class="column-right">
  <ul>
    <li>
      <p>
        がんばらない<br />
        間違ったとかで、push -f をためらわない<br />
        master だけでもいい<br />
        コミットログ英語とかで消耗しない
      </p>
    </li>
    <li>
      <p>
        忘れていい<br />
        気になることは Issue に残して忘れる<br />
        環境構築は README に書いて忘れる
      </p>
    </li>
    <li>
      <p>
        あきらめる<br />
        言語制約などで、そのままでは書けない、とかある<br />
        ex. TypeScript には HashMap 相当の組込クラスがない<br />
        回避策を調べるとか問題自体を変更してしまうとか<br />
        調べる過程で新しい言語の特性などが学べる（重要）
      </p>
    </li>
  </ul>
</div>

---

# こういうところから

```typescript
namespace Money {
  export class Dollar {
    public amount: number = 0;

    constructor(amount: number) {
      this.amount = amount;
    }

    times(multiplier: number): void {
      this.amount *= multiplier;
    }
  }
}

export default Money;
```

---

# こういうところから

```typescript
import assert = require("assert");
import Money from "../src/money";

describe("Money", () => {
  it("Multiplication Test", () => {
    let five: Money.Dollar = new Money.Dollar(5);
    five.times(2);
    assert(10 === five.amount);
  });
});
```

---

# こんな感じに

```
import Expression from "./expression";
import Bank from "./bank";
import Sum from "./sum";

export class Money implements Expression {
  protected amount: number;
  protected cur: string;

  constructor(amount: number, currency: string) {
    this.amount = amount;
    this.cur = currency;
  }

...

  static franc(amount: number): Money {
    return new Money(amount, "CHF");
  }
}
export default Money;
```

---

# こんな感じに

```
import assert = require("assert");
import Bank from "../src/bank";
import Sum from "../src/sum";
import Money from "../src/money";

describe("Money module", () => {
  it("Multiplication Test", () => {
    const five = Money.dollar(5);
    assert(Money.dollar(10).equals(five.times(2)));
    assert(Money.dollar(15).equals(five.times(3)));
  });

...

    bank.addRate("CHF", "USD", 2);
    const sum = new Sum(fiveBucks, tenFrancs).times(2);
    const result = bank.reduce(sum, "USD");
    assert(Money.dollar(20).equals(result));
  });
});
```

---

# 参考

* https://github.com/msfukui/tdd-by-example-for-typescript

---
class: center, middle

# Enjoy, coding!
