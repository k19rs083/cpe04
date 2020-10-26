# README.md

## このファイルについて
このファイルでは、cpe04を利用した中間課題における演習内容について、演習における変更点を中心として記述する。

# 主な変更点
本節では、中間課題において、cpe04に対して行った主な変更点を列挙する。

- 見た目における変更点
    - 背景色および文字色の変更
    - ボタンの色の変更
    - 時間計測中に、残り時間と経過時間が緑色に変化する機能を追加
    - ボタンのラベルを変更

- 機能における変更点
    - 時間計測を一時停止し、途中から再開する機能を追加
    - 計測終了時のアラーム音を、ボタンを押して停止できるように改良
    - 時間をセットした際にアナログ時計が表示されるよう改良

- 機能や見た目とは無関係な変更点
    - リファクタリング
        - 一部変数の命名変更およびコメント追加
        - 変数をvarからletに変更
        - 関数で使われていない引数eの除去
        - htmlタグのlang属性の追加
        - headにtitleタグを追加
    - その他
        - *このファイル* を追加したこと

# 変更点に関する解説
本節では、上記に列挙した変更点のうち、大きな変更を加えた点を取り上げて、詳しく紹介する。

## 見た目における変更

### 色に関する変更
近年のアプリケーションのGUIには、背景色として、黒や濃いグレー、濃い青などを取り入れた「ダークテーマ」の流行が見られる。そこで、本課題においても、ダークテーマの実装を目指して色の変更を試みた。
背景色としては、青みがかったグレーである `#03162b` を利用し、文字色には白に近いグレーである `#dedede` を利用した。完全な黒、および完全な白を使わない理由としては、コントラストを抑えてコンテンツの視認性を高めるねらいがある。
その他、ボタンの色にも手を加えている。

### 時間計測中に、残り時間と経過時間が緑色に変化する機能を追加
見た目におけるもう一つの変更点としては、タイマーが動作している間だけ、残り時間や経過時間の表示を緑色にする機能を追加したことである。
本機能の実装にはjQueryを利用しており、以下のようなコードが追加されている。このコード例では、一つのjQueryセレクタに対して複数の処理を定義している。
なお、タイマーの動作が停止したときに時間表示を緑から白に戻るような処理も追加している。

```javascript 1.8
$("#time").html("経過: " + pastTime + "秒").css("color", "#00FF00");
$("#remain").html("あと: " + restTime + "秒").css("color", "#00FF00");
```

## 機能における変更

### 一時停止機能の追加
身の回りにあるタイマーには、時間計測を一時中断する機能が備わっているのが一般的である。本タイマーにも、同様の機能を追加した。この機能はボタンBに割り当てられている。
処理の内容としては、ボタンBを押したときに、その時点までの経過時間を `pausedPastTime` 変数に記憶させておく。
ボタンAを押して時間計測を再開した際には、 `pastTime` 変数に `pausedPastTime` の値を加算して処理することにより、時間計測が途中から再開できるようにしている。

### リセット時のアナログ時計の表示
ボタンCを押して時間をセットした際に、アナログ時計をmyCanvasに表示させることにより、セットした時間を可視化して確認しやすくした。
実装としては、以下のソースコードをボタンCに追加しただけであり、非常に簡単である。
アナログ時計を表示させる処理は `analog(setTime);` である。
1回だけ実行しても表示自体は可能であるが、秒針の残像が残ってしまう。forループを10回回して処理しているのには、この残像を消去する目的がある。

```javascript 1.8
for (let i = 0; i < 10; i++) {
    analog(setTime);
}
```