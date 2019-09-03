# ExtractionLiveComment
## やりたいこと
　にじさんじの切り抜き自動化してぇ

## 動機
　・にじさんじの長－－－－－－－－－－－いライブ配信から<br>
　　見どころを手軽に簡単に抽出したい。（切り抜き動画の自動化）<br>
　・切り抜き職人が動画まとめてくれるまで待ってられねぇ。<br>
　　とくにエビオのなげぇ配信！！！<br>
　・切り抜かれない面白ポイントもみてえ。<br>
　　でも数時間もある配信をまとめてみるなんて時間的に無理。<br>
　・配信内で何か事件が起きたとき、<br>
　　いくつかのライブ配信のＵＲＬをセットしたら、その事件に。<br>
　　関連することを言ってるシーンを抽出してくれたら最高やん<br>
<br>
## 目標
#### コメント分析
　・コメント率の可視化。どこのシーンがコメント数多いのか、<br>
　　どこの時間で盛り上がったかを「見える化」する。<br>
　・他ライバーさんの名前が頻出するシーンの抽出<br>
　・エビオのライブ配信URLとアルスのURL２つセットして、<br>
　　とあるシーンで盛り上がったら、別視点でのそのシーンもスムーズに見れるようにしたい。<br>
#### 音声テキスト化-分析
　・いくつかのライバーの配信をセットして、自分の知りたい事件に関連する内容を話していないか<br>
　　チェックしたい。<br>
　　例えば、アルスとエビオの「いいこいいこ事件」を、ほかのライバーが配信で<br>
　　喋っていたら、そのシーンも抽出できるといいですよね・・・<br>
#### 画像認識-分析
　・マイクラでライバーがチャットに入力した内容に対して、ほかのライブ配信ではそのチャットにどう<br>
　　反応していたか見れたら最強や・・・。<br>
#### そのほか
　・表示されてるグラフなりヒストグラムの任意の個所にマウスをあてると、<br>
　　そのシーンで頻出する単語リストの一覧と、動画のそのシーンの画像が表示されるような<br>
　　インタラクティブなつくりにしてえなぁ・・・。クリックしたら<br>
　　その動画のシーンに飛ぶような仕組みもくれ・・・<br>
　・最終的にはChromeでアドオン化よろ<br>
　・Pythonおせぇ<br>
  <br>
## 検討
　１．とあるシーンに関する各視点のライブ配信から、そのシーンに関するシーンをスムーズに見たい。<br>
 
　　　１－１　ＡとＢの２つのライブ配信に対して、共通する盛り上がるシーンが一つあったとして、<br>
　　　　　　　盛り上がるシーンは2つとも同じようなコメントの内容の傾向に<br>
　　　　　　　なるだろうから、そこをヒントになんとかできないだろうか…<br>
　　　　　　　（それぞれの動画で時間ごとに頻出単語でクラスタリングして、類似度比較みたいな…）<br>
　　　　　　　<br>
　　　１－２　２つのライブ配信でどれだけ時間がズレてるか把握して、同期をとれば万事解決。<br>
　　　　　　　　１．両ライブ配信で、同じ音声が入ってる場合。<br>
　　　　　　　　　　音声で同じ音声が入った瞬間を基準に同期を取る。<br>
　　　　　　　　　　<br>
　　　　　　　　　　**->->YouTube Data APIで字幕データ抽出してなんとかならんだろうか。**<br>
　　　　　　　　　　<br>
　　　　　　　　２．同じ音声が入ってないマイクラ配信の場合<br>
　　　　　　　　　　マイクラのチャット欄に同じセリフが入った瞬間を基準に同期取る。<br>
　　　　　　　　　　　　(1)数時間の動画をローカル保存して、毎フレーム読み込んで画像認識で文字を検知して、<br>
　　　　　　　　　　　　　「文字の内容」・「文字の画面の位置」・「動画時間」を１レコードとして保持。<br>
　　　　　　　　　　　　(2)抽出されたリストから、最古の時間を除く重複する文字の内容を削除。<br>
　　　　　　　　　　　　　「文字の内容」・「その文字が一番初めに出現した時間」　のリストを作成。<br>
　　　　　　　　　　　　(3)Ａ動画とＢ動画の出力されたリストをそれぞれ比較。<br>
　　　　　　　　　　　　　「文字の内容」が一致するものを検索し、「その文字が一番初めに出現した時間」を<br>
　　　　　　　　　　　　　それぞれの基準点とする。<br>
　　　　　　　　　　　　　<br>
　　　　　　　　　　**->->数時間もあるライブ配信にそこまでやるのくそ時間かかる上に、精度大したことなさそう**<br>
　　　　　　　　　　<br>
　　　　　　　　３．手動で基準をセットする。<br>
　　　　　　　　　　　　　<br>
　　　　　　　　　　**->->クソ有力。**<br>
　　　　　　　　　　　　　<br>
　２．音声のテキスト化できたら色々分析はかどりそうよね<br>
　　　２－１　YouTube Data APIか字幕データを取得してなんとかできんやろか<br>
　　　２－２　独立成分分析でライバーさんの声ごとに音源分離させてぇ・・・<br>
　　　　　　（分離してどうするかは何も考えてない。）<br>
   <br>
　３．任意のシーンのサムネイル取得して、ＵＩに活用してぇ。<br>
　　　３－１　どうやるかまるでわからん。でも、youtubeで再生プレイヤーにカーソル当てたら、<br>
　　　　　　　その地点のプレビューイメージが表示されるから、<br>
　　　　　　　YouTube Player APIにそういう機能あったりしないかなーと期待<br>
       <br>
　　　３－２　なにもかも諦めて、任意のシーンのサムネイルを取得する必要がないものを作成する。<br>
　　　　　　　グラフをクリックしたら、その地点に対応する再生時間を<br>
　　　　　　　YouTube Player APIにセットして、指定時間の箇所でプレイヤーが再生されるよう作る。<br>
　　　　　　　Pythonじゃなく、ブラウザで動くものを作成。<br>
<br>
　４．ＡＰＩによるその他分析も検討しよう<br>
　　　４－１　YouTube Analytics APIの"audienceWatchRatio"で任意の時間の視聴率のようなものを取得できる模様。<br>
　　　　　　　視聴率が高いシーン＝見どころシーンとみなせるかも？<br>
<br>
　５．グラフをクリックしたら●●やる～とかPython<で無理じゃね？<br>
　　　５－１　ＧＵＩライブラリが意外とある。Tkinterがあるだろ。<br>
<br>
　６．チャットの入力内容と入力時間の検知してぇ。<br>
　　　６－１　ライバーがマイクラチャットで入力した内容と入力時間を正確に検知できれば、<br>
　　　　　　　他ライバーの視点でそのチャットにどう反応してたかすぐに見られる。すばらしい！<br>
　　　　　　　どうやるかは知らんがなんとかしてくれぇ！１－２－２に書いたようなノリで、リスト化できないやろか<br>
　　　　　　　　　　**->->そこまで細かい情報は不要。**<br>
　　　　　　　　　　**->->１－２で他動画との同期が取れてる状態なら、**<br>
　　　　　　　　　　**->->チャットが入力された瞬間を検知すれば、それで十分。**<br>
　　　　　　　　　　**->->キーボードの音を機械学習で学習させて、検知させよう！（どうやるかは知らん）**<br>
 <br>
<br>
　７．い、いろいろ書いてるけど、がっつりＵＩが必要な感じですよね。どんなＵＩを想定しておいでですか。<br>
　　　７－１　**１－モード：コメント分析モード**<br>
　　　　　　　画面構成　(1)コメント率が可視化されてるグラフ<br>
　　　　　　　　　　　　(2)「コメント全体」「各ライバーの名前」で切り替えられるスイッチ<br>
　　　　　　　　　　　　(3)動画プレイヤー<br>
            <br>
　　　　　　　初期状態：「コメント全体」のコメント率をグラフ表示<br>
　　　　　　　クリック：**【グラフ】**<br>
　　　　　　　　　　　　　そのグラフの点に対応する再生時間で動画が再生される<br>
　　　　　　　クリック：**【スイッチ】**<br>
　　　　　　　　　　　　　「コメント全体」「各ライバーの名前」いずれかのコメント率を表示<br>
       <br>
       <br>
　　　　　　　**２－モード：カプ厨歓喜！分析モード**<br>
　　　　　　　画面構成　(1)メインとなる配信ＵＲＬを入力するテキストボックス<br>
　　　　　　　　　　　　(2)サブとなる配信ＵＲＬを入力するテキストボックス<br>
　　　　　　　　　　　　(3)メインとなる配信のコメント率グラフ<br>
　　　　　　　　　　　　(4)チャット履歴表示スペース<br>
　　　　　　　　　　　　(5)別視点表示スペース<br>
　　　　　　　　　　　　(6)検索用テキストボックス<br>
　　　　　　　　　　　　(7)検索ボタン<br>
　　　　　　　　　　　　(8)検索結果表示スペース<br>
　　　　　　　　　　　　(9)動画プレイヤー<br>
            <br>
　　　　　　　初期状態：メイン配信に対する「コメント全体」のコメント率をグラフ表示<br>
　　　　　　　　　　　　表示スペースに、マイクラチャットの入力履歴を表示。<br>
　　　　　　　クリック：**【グラフ】**<br>
　　　　　　　　　　　　　クリックした点に対応する再生時間で動画再生。<br>
　　　　　　　　　　　　　別視点表示スペースに、メイン配信のに<br>
　　　　　　　　　　　　　対応するサブ配信の再生リンクを表示し、<br>
　　　　　　　　　　　　　リンクをクリックすると、そのシーンの別視点が再生される。<br>
　　　　　　　クリック：**【マイクラチャット履歴】**<br>
　　　　　　　　　　　　　メイン・サブ配信の各支店の再生リンクをツリーで一覧表示。<br>
　　　　　　　　　　　　　それらのリンクを押すと、そのチャットに対する各支店の動画を再生。<br>
　　　　　　　クリック：**【検索ボタン】**<br>
　　　　　　　　　　　　　検索ボタンをクリックしたら、検索用テキストボックスに<br>
　　　　　　　　　　　　　入力された内容を、ライバーの音声テキスト化データから検索し<br>
　　　　　　　　　　　　　関連したこと喋っているシーンの再生リンクを検索結果表示スペースに表示。<br>
       <br>
<br>
## 進捗
　　　20190903：ライブコメント取得して、特定の単語の出現回数をヒストグラムに出す程度しか作ってないよ！<br>
<br>
## 参考にしたやつ
下記サイトでライブコメントを抽出する処理をまんま引用。<br>
https://qiita.com/okamoto950712/items/0d4736c7be251532a03f<br>
