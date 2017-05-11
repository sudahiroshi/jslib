# jslib

ファイル校正

1. nylon.js : イベントエミッタ
1. vbtimer.js : タイマーによるバックグランド処理
1. vbcanvas.js : 仮想座標機能付きcanvas

## nylon.js

イベントエミッタです．イベントが起きたらやりたいことを記述しておき，例えばボタンがクリックされたらイベントを起こすようにして，イベントドリブンなプログラムを組みやすくします．

### コンストラクタ

パラメータ無しで呼び出し，イベントエミッタを初期化します．

```javascript
var nl = new nylon();
```

### on

イベントキーワードとコールバック関数を指定します．
イベントは文字列で与えます．
コールバック関数は，パラメータをハッシュで受け取るように記述してください．

```javascript
nl.on( "test", ( key, params ) => {
  console.log( "送られて来た内容は" + params["foo"] + "です" );
});
```

### emit

イベントキーワードとパラメータを指定して，イベントを発生します．
キーワードはパイプ記号で区切り，複数指定できます．
なお，nylonオブジェクトが同一の場合，イベントをスルーします．
別のnylonオブジェクトを作成してください．

```javascript
var nl2 = new nylon();
nl2.emit( "test", { foo: "「これが本文」"} );
// onのところで指定したコールバック関数が実行され，下記が表示される
// 送られて来た内容は「これが本文」です
```

```javascript
// 複数のイベントキーワードを指定する例
// ここでは，test，bar，bazの3つを指定している
var nl2 = new nylon();
nl2.emit( "test|bar|baz", { foo: "「これが本文」"} );
// 実行結果は上と同じ．
// 他にbarやbazを受け取るonを記述していた場合はそれらの結果が追加される
```

### emitByArray

イベントキーワードを配列で渡すemitです．

```javascript
// 複数のイベントキーワードを指定する例
// ここでは，test，bar，bazの3つを指定している
var nl2 = new nylon();
nl2.emit( ["test", "bar", "baz"] , { foo: "「これが本文」"} );
```

## vbtimer.js

タイマーによるバックグランド処理を支援するライブラリです．
JavaScriptに用意されているsetIntervalでは，一時停止・再起動が面倒だったので作りました．

### コンストラクタ

パラメータ無しで呼び出し，タイマーを初期化します．

```javascript
var timer = new vbTimer();
```

### interval変数

動作感覚を設定します．
単位はミリ秒で，初期値は1000msec（1秒）です．

```javascript
timer.interval = 1000; // 動作感覚を1000msec（1秒）に設定
```

### timer変数

バックグラウンドで行う処理を記述します．
functionかラムダ式で与えてください．

```javascript
timer.timer = () => {
  console.log( new Date() );  // 時刻を取得して表示
}
```

### enable

タイマーの動作を開始または再起動します．

```
timer.enable();
// コンソールに1秒毎に時刻が表示されるようになります
```

### disable

タイマーの動作を一時的に止めます

```javascript
imer.disable();
// 1秒毎の時刻表示が止まります
```
