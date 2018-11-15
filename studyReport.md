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
#### 手法1: 片側にmarginをとる  
親widthが500pxとして  
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
