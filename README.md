# node-red-ogiri-01

こちらのNode-REDフローは、Node-RED UG Japanイベント「Node-RED大喜利」の第一回目のお題に対する解の一つになります。
https://node-red.connpass.com/event/257963/

## フロー
フロー全体のイメージはこんな感じで仕上げました。
<img width="821" alt="Screen Shot 2022-09-29 at 16 03 19" src="https://user-images.githubusercontent.com/12064399/192963127-dd1648c6-6a8e-45c7-ba31-205ac083fcc9.png">

フローのJSONファイルは(こちら)[https://github.com/taijihagino/node-red-ogiri-01/blob/main/flows.json]から取得いただけます。

## 各ノードの解説
まずInjectノードです。こちらはデフォルトのままタイムスタンプを送信します。

<img width="552" alt="Screen Shot 2022-09-29 at 16 03 36" src="https://user-images.githubusercontent.com/12064399/192963258-d437e594-71de-46ee-8166-9bacfe265406.png">

次に、Injectノードで出力された日時をChangeノードを使ってGlobal変数へ入れておきます。
以下の式でYYYYMMDDにフォーマットしておきます。（これは読み込むCSVに持っている日付のフォーマットに合わせましょう）
```
$fromMillis($millis(),'[Y0001]/[M01]/[D01]')
```

<img width="524" alt="Screen Shot 2022-09-29 at 16 03 47" src="https://user-images.githubusercontent.com/12064399/192963428-c47fce42-e2b0-467a-ac7a-422cdcbfec33.png">

<img width="526" alt="Screen Shot 2022-09-29 at 16 04 08" src="https://user-images.githubusercontent.com/12064399/192963460-ef21e95a-c9f1-4204-9b72-b75270080e32.png">

Web上に公開されているCSVファイルをhttp requestノードを使って読み込みます。
ここでは敢えてGitHubではなく、別のサーバー上にCSVファイルを置いて、それにアクセスするようにしています。

<img width="525" alt="Screen Shot 2022-09-29 at 16 04 20" src="https://user-images.githubusercontent.com/12064399/192963974-0eef1b94-4634-46b9-8f92-4f8df470b996.png">

csvノードを使ってパースします。 今回読み込むファイルにはヘッダ行が無いので 「1行目に列名を含む」のチェックは外してあります。

<img width="580" alt="Screen Shot 2022-09-29 at 16 04 35" src="https://user-images.githubusercontent.com/12064399/192964174-f759dee4-295f-4053-b5a1-9a0500330ed4.png">

splitノードで1行1行を配列にしていきます。配列にしておくことで、その後のアウトプットの方法に合わせてデータを扱いやすくしています。

<img width="520" alt="Screen Shot 2022-09-29 at 16 04 56" src="https://user-images.githubusercontent.com/12064399/192964825-449fbfea-f881-456b-aeb6-1d255ec474e3.png">

switchノードを使い、global変数に格納しておいた本日日付の値を元に配列のpayloadの中身を評価します。
条件に合致した場合のみ次のDebugノードへ出力します。

<img width="527" alt="Screen Shot 2022-09-29 at 16 05 07" src="https://user-images.githubusercontent.com/12064399/192965367-30c2dd68-8180-4387-8f5d-87e9912c0ccc.png">

このような感じで1行づつDebugノードへ出力されます。
ファイル書き込み、DB書き込み、Web画面へ出力、など、扱いやすい形でCSVから条件に合致した行を抜き出せたのでは無いでしょうか。

<img width="653" alt="Screen Shot 2022-09-29 at 16 05 30" src="https://user-images.githubusercontent.com/12064399/192965642-ec35031e-23f0-4954-8235-585845c819f9.png">

## まとめ
この方法はあくまで数あるやり方の内の一つにすぎません。自分でもっとやりやすい方法が見つけられればその方が良いでしょう。
いろいろと試してみてくださいね！
