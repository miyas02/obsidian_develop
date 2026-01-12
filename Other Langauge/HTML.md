---

---
```css
/*containerクラスを中央に配置*/
.container{
	width:300px;
	margin:0 auto;
}

/*top-wrapperに背景画像を配置*/
.top-wrapper{
	height:200px;
	background-image:url();
	background-size:cover; /*大きさを自動調整*/
}

/*top-wrapperの中のh1を指定*/
.top-wrapper h1{
	opacity:0.7; /*透明度を指定*/
	letter-spacing:5px;/*文字間隔の指定*/
}

/*ボタンの作成 <a>タグのdisplayの変更*/
.btn{
  padding:8px 24px;
	background-color:blue;
  color:white;
  display:inline-block; /*block,inline-block,inline*/
  opacity:0.8;
	border-radius:4px;/*ボタンの角を丸くする*/
}
/*ボタンにカーソルが乗った時の処理*/
.btn:hover{
	opacity:1;
}
/*Font Awesome手順
①Font AwesomeのCSSファイルを読み込む
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
②<span>タグにfaクラスとfa-アイコン名クラスを読み込む
*/
/*HTML*/
<a href="" class="btn facebook">
<span class="fa fa-facebook"></span>
facebook
</a>

/*rgba その色だけ透過*/
header{
	background-color:rgba(34,49,52,0.9);
}

/*aタグをdisplay:blockにするとブロックがリンクになる*/
.header-right{
	display:block;
}

/*アニメーション　transition:all  1s;*/
/*文字の太さ*/
.heading h2{
	font-weight:normal;/*normal,bold*/
}

/*widthやheightを%で指定すると、親要素に対しての割合で指定*/
.lesson{
  float:left;
  width:25%;
}

/*position:absolute;
要素を重ねる
親要素にposition:relative;を指定
子要素にposition:absolute;を指定
relative基準からの場所に子要素を配置
*/
.lesson-icon {
  position: relative;
}

.lesson-icon p {
  position: absolute;
  top: 90px;
  width: 100%;
  color: white;
}

/*影をつける*/
.message {
  padding: 15px 40px;
  background-color: #5dca88;
  /* box-shadowをつけてください */
  box-shadow:0 7px #1a7940;
}

/*クリックされたときの処理 activeをつける*/
.message:active{
  box-shadow:none;
  position:relative;
  top:7px;
}

/*画面上に要素を固定*/
header {
  height: 65px;
  width: 100%;
  background-color: rgba(34, 49, 52, 0.9);
  /* positionプロパティをfixedに、topを0に指定してください */
  position:fixed;
  top:0;
  
  /* z-indexを10に指定してください レイヤ優先*/
  z-index:10;
}

/*flex*/
/*親要素に対してdisplay:flexを指定
.flex-list {
  display: flex; 
}
/* .flex-list liに、flexプロパティを指定してください */
.flex-list li{
  flex:auto;
}
```

float 子要素に対して指定

font awesome使い方 [https://www.webdesignleaves.com/pr/plugins/fontawesome_03.html](https://www.webdesignleaves.com/pr/plugins/fontawesome_03.html)

 [Host Yourself Web Fonts](https://fontawesome.com/docs/web/setup/host-yourself/webfonts)
上からfileをダウンロード
cssとwebfontsフォルダをコンテキスト直下に配置
htmlに`<``**link**`` href="/shop/jsp/css/all.min.css" rel="stylesheet">`と記述
`<``**i**`` class="fa-solid fa-cart-shopping"></``**i**``>`など使いたいフォントを検索してhtml内に記述
