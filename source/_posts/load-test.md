---
title: 負荷テストツール
date: 2019-11-18 10:32:08
tags: ['load test', 'development', 'JMeter', 'Vegeta']
---

## 負荷テストを行うためのツール

いままでJMeterか、簡単に行うならApach Benchぐらいしか選択肢がなかった負荷テストツールですが、最近いくつか新しいツールを教えてもらったので、忘れないようにメモ。
こういう面にもアンテナを伸ばしていかないと、エンジニアとして後れを取ってしまうなぁと感じる今日この頃。そのうち、ここに焦点を当てた記事も書いていきたい。

### JMeter

負荷テストといったらこれ。といった印象。というか、最近までこれ以外の方法をろくに知らなかった（apach benchをのぞく）。
macで作ったシナリオをubuntuに持って行ったとき、バージョンが合わずに動かなくて苦労したことがある。GUIが使える環境かどうかでJMeterのバージョンも変わる。その時は結局ubuntu側に諸々入れてmacのバージョンに合わせた。そんなubuntuのインスタンスを各リージョンに立てて、システムにアクセスさせるテストを行ったことがある（AWS）。

https://jmeter.apache.org/

### Vegeta

最近知った負荷テストツール。Golang製であるのが売りだろうか。つまりJavaじゃない。

https://github.com/tsenart/vegeta

### Gatling

軽量でスケールアウトし、スクリプト（Scala）でシナリオ（テスト）をつくれてxmlがいらないやつ。でもJDK8以上が必要。

https://gatling.io/

### Tsung

動作が軽い。付属ツールでHTMLでのレポートを出力可能。スレッド数での負荷調整ではなく、秒間〇セッションといった風な調整方法なので、そのまま秒間〇アクセスといった目標値を設定しやすい。
