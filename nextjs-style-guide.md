# Nextjs プロジェクト コーディング規約

- Next プロジェクトにフォーカスを当てています(一部省略)
- 開発途中のため、適宜変更されることが予想されます

## 全体共通

### フォルダ構成

- /
  - /.next : ビルドしたファイルの置き場（自動生成のため、手動で更新しないこと）
  - /out : ビルド内容をもとに吐き出される静的ファイルの置き場（自動生成のため、手動で更新しないこと）
  - /public : 写真などの素材置き場、`public`以下は`/hoge`でアクセスできる
  - /src : 開発者が触るソースコードをまとめる場所
    - /components : ユーザー定義のコンポーネント群
      - /HogeFuga : コンポーネント
        - index.tsx : コンポーネントの中身
        - HogeFuga.module.scss : コンポーネントのスタイルシート
      - index.ts : コンポーネント群のエントリーポイント
    - /hooks : カスタムフック群
      - useHoge.ts : `use`で始める
    - /utils : 関数などの処理ファイル群
    - /pages : page 群
    - /styles : page のスタイルシート群
    - types.ts : 型情報群（自動生成のため、手動で更新しないこと）
  - next-config.json : Next.js のカスタムする設定を書くファイル

### コメント

規約から外れた書き方をする場合、他の人が理解し難いと予想される部分にはコメントを残す

### インデント

スペースを２つ使う

### 1 行 1 要素

可読性を担保するため１行に書く要素は１つに留める（js 部分はこの限りではない）

```html
<!-- Recomended -->
<ul>
  <li>itemA</li>
  <li>itemB</li>
</ul>

<!-- Not Recomended -->
<!-- prettier-ignore -->
<ul>
  <li>itemA</li><li>itemB</li>
</ul>
```

```css
/* Recommended */
.selector {
  font-size: 1.2em;
  color: #a1a1a1;
}

/* Not recommended */
/* prettier-ignore */
.selector {
  font-size: 1.2em;color: #a1a1a1;
}
```

### 大文字小文字

タグ、ID、クラス、色コードなどの記述は DOCTYPE、キャメルケース使用の際の大文字を除き常に小文字を使う。

```css
/* Recommended */
.selector {
  color: #a1a1a1;
}
/* camelCase */
.mySelector {
}

/* Not recommended */
.SELECTOR {
  color: #a1a1a1;
}
.MYSELECTOR {
}
```

## HTML

### 段落とヘッダー

`<p>`タグの中に`<h1>`などのヘッダーを使用しない。

```HTML
<!--Redomended-->
<h1>Header</h1>
<p>lorem ...</p>

<!--Not redomended-->
<p>
  <h1>Header</h1>
  lorem ...
</p>
```

### 外部リンク

- 外部にリンクしている、`<a>`タグに`rel=”noopener noreferrer”`を記述する。

```HTML
<!--Redomended-->
<a href=”http://www.hogehoge.com” target=”_blank” rel="noopener noreferrer">リンク</a>

<!--Not Redomended-->
<a href=”http://www.hogehoge.com” target=”_blank”>リンク</a>
```

## CSS(SASS)

### プロパティ記述の際のスペース

プロパティ: \_ 設定値;のようにスペースを利用する。（sass では特に）

```css
/* Recommended */
.nav-bottom
  color: red

/* Not recommended */
.nav-bottom
  color:red
```

### プロパティの記述順序

基本的にアルファベット順に記述する。

```css
#overlay {
  background: #fff;
  color: #777;
  padding: 10px;
  position: absolute;
  z-index: 1;
}
```

### !important は使用しない

使用しなければならない場合は HTML の構造を見直す。極力使わない

## JS(React)

### 変数と関数

キャメルケースを使う

```JavaScript
// Recommended
const fooVar;

// Not Recommended
const FooVar;
```

### 定数

完全に固定値のものを宣言する時の記述
環境変数などもこれに準じる

```JavaScript
// Recommended
const MAX_SIZE = 10;

// Not Recommended
const MaxSize = 10;
```

### ユーザー定義のコンポーネント

- 関数コンポーネントにパスカルケースを使う
- function ではなくアロー関数を使う
- named export を用いる
- エントリーポイントに追記する
  - `/src/components/index.ts`

```JavaScript
export const FooItem = () => {};
```

### ページの定義方法

- function ではなくアロー関数を使う
- default export を使う

### 文字連結

テンプレートリテラルを用いる

```JavaScript
// Recommended
const apiUrl = `${origin}/api/v2/user/${userId}`;

// Not Recommended
const apiUrl = origin + '/api/v2/user/' + userId;
```

### if の使用を避ける

- 基本的には短絡評価を使用する
- 記述が長くなる、分岐が複雑化する場合には if や switch を使用する

```JavaScript
// Recommended
  return status == 'JPN' ? <JapaneseHeader /> : <EnglishHeader />;

// Not Recommended
if (status == 'JPN') {
  return <JapaneseHeader />;
} else {
  return <EnglishHeader />;
}
```

### render 内イベント命令系は切り離す

- 基本的にビューとロジックを切り離す
- 必要であればファイルを分ける

```JavaScript

// Recommended
const displayConsole = (e) => {
  console.log("target:", e.target);
};

return <Button onClick={displayConsole}>Push</Button>;

// Not Recommended
  return (
    <Button
      onClick={(event) => {
        console.log(event.target);
      }}
    >
      Push
    </Button>
  );
```

### 使い回す可能性のある処理は関数化して切り分ける

- 基本的にビューとロジックを切り離す
- 必要であればファイルを分ける
- 例）データのフェッチ、時刻の整形処理

### マジックナンバー(数値そのものの意味以外を含んでいない具体的な数値)の使用を控える

- 数値の意味が理解し難くなるため
- 数値が関係する箇所を見極めるのが困難になる
- 数値に対して意味のあるラベリングを行う

```JavaScript

// Not Recommended
const checkAge = (age: number) => {
  if(age < 18){
    console.log('未成年です');
  }
  else{
    console.log('成人です');
  }
};

// Recommended
const ADULT_AGE = 18;
const checkAge = (age: number) => {
  if(age < ADULT_AGE){
    console.log('未成年です');
  }
  else{
    console.log('成人です');
  }
};

```

### 変数のスコープ範囲は必要最小限にする

- 必要以上にスコープを広くすると様々な関数に干渉してしまってバグを生んでしまうことがある
- また、グローバル変数は極力使用しない

## 参考にした記事

- https://qiita.com/WalkerWalks/items/9c9a1098404cd89c0068
- https://www.1st-net.jp/blog/coding-regulation/#_target_blank
- https://ja.wordpress.org/team/handbook/coding-standards/wordpress-coding-standards/css/
- https://typescript-jp.gitbook.io/deep-dive/styleguide
- https://qiita.com/JNJDUNK/items/e040925e546a45a1d913
- https://raychonote.com/markdown/dictory-list.html
