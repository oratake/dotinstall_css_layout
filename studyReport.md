# CSSレイアウト
ドットインストール、[CSSレイアウト入門](https://dotinstall.com/lessons/basic_css_layout)の学習レポートです  

## スタイリング前の地ならし
ブラウザには**User Agent Stylesheet**というCSSが設定されており、  
ブラウザを問わず同じ見た目にするためには、  
- reset.css  
- nomalize.css  
などを使う必要がある。  
dotinstallでは[html5doctor](http://html5doctor.com/html-5-reset-stylesheet/)のreset.cssを利用することを推奨している。  
※弊社では既定のものを使用。  

使用する順は、  
1. CSSを均して  
1. 自分のCSSをあてる  

なので、**ページのcssより先に**指定する必要あり。  

## スタイリングのテクニック集

### ブロック要素の中央揃え
`.要素 { margin: 0 auto; }` 横マージンをautoでとる。  

### 背景は画面いっぱい、内容は少し余白もたせたい
要素のなかに、もう一つ要素を入れる。
```html
<main> <!-- 背景はこっちに指定 -->
  <div class="container">
    <p>余裕もたせたい内容はこっちに指定。CSSで後は頑張る。</p>
  </div><!-- /.container -->
</main>
```

### footerの背景色を下まで伸ばしたい
footerを伸ばすのもありだが、bodyにfooterと同じ色を指定してしまうのも手  

### 背景画像を横幅いっぱいに表示したい(アイキャッチ画像)
```html
<div id="eyecatch">
  <div class="container">
    header
  </div>
</div>
```
```css
#eyecatch {
  background: url('eyecatch.jpg') no-repeat; /* no-repeat で画像を何枚も表示しないように。*/
  background-size: cover; /* coverで横幅いっぱいの表示 */
  background-position: 50%; /* 画像の中心をどこにするか。x,y方向に50% */
  height: 100px;
}
```

### 2カラムのレイアウト(float)
floatを用いて、左にどちらも寄せる方法。↓あてるhtml  
```html
<main>
  <div class="container">
    <div id="left">左カラム</div>
    <div id="right">右カラム</div>
  </div>
</main>
<footer>フッター</footer>
```
#### 手法1: 直後にclear: both;する方法
1. 並べたい要素(#left, #right)を`float: left;`し、widthを指定。
```css
#left {
  float: left;
  width: 200px;
}
#right {
  float: left;
  width: 300px;
}
```
こうすると横に並ぶが、下の要素(footer)が回り込んでしまう。  
1. 続く要素に`clear: both;`を指定し、回り込みを解除
```css
footer { clear: both; }
```
解除されたが、親要素mainに背景色や画像がある場合、floatされているものは計算外なので、heightが0になってしまう。(=表示されない)  
1. float部分の親要素の中にfloatしない要素を入れて、親要素の高さを出す。  
```html
<main>
  (2カラムここ)
  <div style="clear: both;"> <!-- 実際はstyle属性などではなくcssに。 -->
</main>
```

#### 手法2: もっとスマートに。親要素でclearfixする
上記の方法では、高さ出しのために空の要素を指定しているので、分かりづらい。  
そのため、::after 疑似要素で`clear: both;`する。  
1. .clearfixクラスを親要素に指定  
```html
<div class="container clearfix">
  <div id="left">左カラム</div>
  <div id="right">右カラム</div>
</div>
```
2. `.clearfix::after`疑似要素に`clear: both;`を指定
```css
.container::after {
  content: ''; /* ::afterは何かしらcontentが必要なので、空要素を指定 */
  display: block; /* ブロック要素として指定 */
  clear: both;
}
```

#### 手法3: もっとさらにスマートに。親要素にoverflow: hidden;する
```html
<div class="container">
  <div id="left">左カラム</div>
  <div id="right">右カラム</div>
</div>
```
```css
.container { overflow: hidden; }
```
`overflow: hidden;`は、要素の領域からはみ出した部分の処理を既定するもので、hiddenはその部分を非表示にしてしまうというもの。  
ブラウザはoverflowを指定すると、floatした子要素の高さを認識するので、こうしている様子。  

### 2カラムに余白をつける
親widthが500pxとして 
#### 手法1: 片側にmarginをとる   
```css
#left {
  float: left;
  width: 180px;
  margin-right: 20px;
}
#right {
  float: left;
  width: 300px;
}
```
#### 手法2: floatでleftとrightにそれぞれ振る
```css
#left {
  float: left;
  width: 190px;
}
#right {
  float: right;
  width: 290px;
}
```

#### 手法3: paddingを取る
1. 双方のカラムにpaddingを設ける
```css
#left, #right { padding: 10px; }
```
ただし、こうするとそれぞれのwidthにpadding分が足されて、横に収まらずに溢れる。ので、  
2. `box-sizing: border-box;`でwidthやheightに`border`,`padding`を含めるようにする
```css
#left, #right {
  padding: 10px;
  box-sizing: border-box;
}
```

### 可変幅の2カラムをつくる
widthに%の値を指定すると作れる。  
※親要素のwidth指定は解除した  
#### どちらも可変幅
```css
#left, #right { float: left; }
#left { width: 30%; }
#right { width: 70%; }
```
#### 片方が可変幅
#rightを固定、#leftを可変にする。  
#rightをfloatで右に固定、#leftはfloatせず、#right分のmarginをとる。  
**注意**：floatで浮かせる要素は**先に**書かねばならない。  
後に書いてしまうと表示が崩れるので、右側(#right)をfloatにするには、  
**floatさせる右側を先に**書く必要がある。  
```html
<div class="container">
  <div id="right">右</div> <!-- float要素は先に -->
  <div id="left">左</div>
</div>
```
```css
#left {
  margin-right: 200px;
}
#right {
  float: right;
  width: 200px; /* 幅固定 */
}
```

### メニューのスタイル( ul > li )
ulでつくることが多い。  
```html
<ul id="menu">
  <li><a href="">Menu 1</a></li>
  <li><a href="">Menu 2</a></li>
  <li><a href="">Menu 3</a></li>
</ul>
```
#### 余計な装飾を取る
```css
ul#menu {
  padding: 0; /* 余計な余白をとる */
  margin: 0;
  list-style-type: none; /* 点を消す。色々指定もできる。 */
}
ul#menu > li > a {
  text-decoration: none; /* リンクの下線を消す */
}
```

#### 要素の大きさを変える
aタグにサイズ指定をする。  
と自分の文字しかリンクにならないので、ボタン全体をリンク指定するために、  
aタグにもinline-blockを指定する。  
```css
ul#menu > li > a {
  display: inline-block;
  width: 80px;
  height: 40px;
}
```

#### 要素の並び、文字揃え
inline-blockで要素を横並びにする。
```css
ul#menu > li { display: inline-block; }
```
文字を中央に寄せる  
```css
ul#menu > li > a {
  height: 40px;
  line-height: 40px; /* heightと一緒の値にすることで垂直方向を中央寄せ */
  text-align: center; /* 水平方向の中央寄せ */
}
```
要素同士の隙間が空いているので、以下を追加して解決する。  
```css
ul#menu { font-size: 0; }
ul#menu > li > a { font-size: 16px; } /* ここで正しいfont-sizeを指定する */
```

#### マウスオーバーでデザイン変更
カーソルが乗ったら、ゆるやかに色が変化するようにする。  
今回の色の変化は透明度を使用。  
```css
ul#menu > li > a:hover {
  opacity: 0.5; /* 0-1の透明度で変化を指定 */
  transition: 0.3s; /* 指定時間かけて変化 */
}
```
