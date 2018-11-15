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
ので、**ページのcssより先に**指定する必要あり。  

