+ 元文書: [stylus/docs/conditionals.md at f9ed220d7e5f0b44aeaca58ffd6490566b5f0757 · LearnBoost/stylus · GitHub](https://github.com/LearnBoost/stylus/blob/f9ed220d7e5f0b44aeaca58ffd6490566b5f0757/docs/conditionals.md)

## Conditionals

## 条件 [原文](http://learnboost.github.com/stylus/docs/conditionals.html)

 Conditionals provide control flow to a language which is otherwise static, providing conditional imports, mixins, functions, and more. The examples below are simply examples, and not recommended :)

 条件は静的な言語にフロー制御を提供し、条件によるインポート、ミックスイン、関数などを可能にします。以下の例は単なる例であり、お勧めはしません :)

### if / else if / else

 The `if` conditional works as you would expect, simply accepting an expression, evaluating the following block when `true`. Along with `if` are the typical `else if` and `else` tokens, acting as fallbacks.

 `if` 条件はあなたの予想通りの動作をします。言うまでもなく、式の受理と、暗黙的な `true` ブロックの評価が可能です。`if` が典型的なのと同じく `else if` と `else` トークンも同じく、加えてフォールバックとして機能します。
 
 The example below would conditionally overload the `padding` property, swapping it for `margin`.

 下記の例では条件付きで `padding` プロパティに負荷をかけ、`margin` と取り換えます。

    overload-padding = true

    if overload-padding
      padding(y, x)
        margin y x

    body
      padding 5px 10px

Another example:

別の例:

    box(x, y, margin = false)
      padding y x
      if margin
        margin y x

    body
      box(5px, 10px, true)

Another `box()` helper:

別の `box()` ヘルパー:

    box(x, y, margin-only = false)
      if margin-only
        margin y x
      else
        padding y x

### unless

 For users familiar with the Ruby programming language, we have the `unless` conditional. It’s basically the opposite of `if`—essentially `if (!(expr))`.

 Ruby プログラミング言語に慣れたユーザーのために、`unless` 条件を用意しています。基本的には `if` の相対で、本質的には `if (!(expr))` です。

In the example below, if `disable-padding-override` is `undefined` or `false`, `padding` will be overridden, displaying `margin` instead. But if it’s `true`, `padding` will continue outputting `padding 5px 10px` as expected.

次の例に示す通り、`disable-padding-override` が `undefined` か `false` であれば、`padding` は上書きされ、`margin` が代わりに表示されます。しかし `true` であれば、`padding` は期待通り `padding 5px 10px` を引き続き出力します。

     disable-padding-override = true

     unless disable-padding-override is defined and disable-padding-override
       padding(x, y)
         margin y x

     body
       padding 5px 10px

### Postfix Conditionals

### 倒置条件

  Stylus supports postfix conditionals. This means that `if` and `unless` act as operators; they evaluate the left-hand operand when the right-hand expression is truthy.

  Stylus は倒置条件に対応しています。つまり `if` や `unless` は演算子として働き; 左手の数値は右手の式が真である時に評価されます。
  
  
  For example let's define `negative()` to perform some basic type checking. Below we use block-style conditionals:

  例えば `negative()` を定義したとして基本的な型確認をいくつか実行します。次のようなブロックスタイルの条件を用います:
  
      negative(n)
        unless n is a 'unit'
          error('invalid number')
        if n < 0
          yes
        else
          no

  Next, we utilize postfix conditionals to keep our function terse.

  次に、倒置条件を関数の簡明さを保つために用います。

      negative(n)
        error('invalid number') unless n is a 'unit'
        return yes if n < 0
        no

  Of course, we could take this further.  For example, we could write `n < 0 ? yes : no`, or simply stick with booleans: `n < 0`.

  もちろん、さらに短かくする事もできるでしょう。例えば、`n < 0 ? yes : no` やブール代数で: `n < 0`

  Postfix conditionals may be applied to most single-line statements. For example, `@import`, `@charset`, mixins—and of course, properties as shown below:

  倒置条件はたいてい 1 行ステートメントで使用するでしょう。例えば、`@import`、`@charset`、ミックスインとそしてもちろん、次に示すようなプロパティで:

  
  
      pad(types = margin padding, n = 5px)
        padding unit(n, px) if padding in types
        margin unit(n, px) if margin in types

      body
        pad()

      body
        pad(margin)

      body
        apply-mixins = true
        pad(padding, 10) if apply-mixins

yielding:

イールド:

      body {
        padding: 5px;
        margin: 5px;
      }
      body {
        margin: 5px;
      }
      body {
        padding: 10px;
      }

