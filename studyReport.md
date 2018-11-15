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
