Imoten-Wiki
===========
Imotenの設定に関するページは数あれど、SIMフリー機のAndroid機での絵文字（Docomo)
携帯メールの最適な設定方法については
ほとんど記されていないため、チラ裏的に、ここにメモッておく。
とりあえず、この通りにやればiMoniもSPモードメールも全く必要なくなる。
※ログイン通知メールでパンクすることも無くなる（はず。）
というかほとんど、「くずのは探偵事務所」のHP http://www.kyoji-kuzunoha.com/の内容ををパクったのだが
Androidに最適化していないため、我流の部分を加えただけの自分用メモ

用意するもの
1.SIMフリーのAndroid機（要root化）
2.ガラケー(DoCoMo機)とDoCoMoのSIMカード
　(白SIM xi音声プランが一番安く済む。つうかパケホでヤルとコスパ悪いから緑SIMならDSで取り替えてこい）
3.libemoji_docomo.so(DoCoMoのスマフォからかっぱらてくる）
4.ATOK
5.ターミナルソフト（なんでもいいがWindowsだとTera Termがおすすめ）
http://www.forest.impress.co.jp/lib/inet/servernt/remote/utf8teraterm.html
6.Androidの「メール通知」ソフト
https://play.google.com/store/apps/details?id=net.assemble.mailnotify

手順
1.ガラケーでi-modeメールの設定とiモード.netの設定を済ませておく
　※申し込み方法、設定方法などはググれば腐るほどでてくるので、ここでは書かない

2.次にmoperaスタンダードの契約を行い、mopera.netのメールアカウントを取得する。
　※Gmailや他のメールでもいいのでだが、Moperaメールは完全Ppsh型なため、データ通信を切っていても
　電話のアンテナさえ立っていればpush信号が送られてくるため、非常に電池にやさしい
　というか、MoperaにしないならiMoni運用した方がはるかに楽チン
　※これの最大唯一のメリットはデータ通信を切っていてもpushでメールの受信ができることにある。
　
3.root化したAndroid機にlibemoji_docomo.soをぶち込む
  adb push libemoji_docomo.so /system/lib/libemoji_docomo.so
  adb shel
  cd /system/lib
  chown 0.0 libemoji_docomo.so
  chmod 644 libemoji_docomo.so
4./system/build.propの適当な所に ro.config.libemoji=libemoji_docomo.so と記述し再起動

5.ATOKをインストールし、ATOKの設定から携帯電話事業者の選択でNTTドコモを選択、絵文字を常に利用にチェック

 これでSIMフリー機でDoCoMoの絵文字が表示＆入力可能になる。

6.VPSの安いサーバを借りる。※自前のPCでVPS立ててもいいが、管理がめんどくさいので借りるのが楽だしオススメ 
　一番安いのは490JPY/月で借りれるDTIか。http://dream.jp/vps/
 OSは32bit版CentOS5を借りる事を前提で話をすすめる。
 クレカ打ち込んで申し込みしたら、すぐにメールでホストのIPアドレスとrootのパスワードが発行される（超早い）
 
7.Windows機からTeraTermを起動しDTIから発行されたIPアドレスとポートを打ち込んでOKボタン
 ログイン画面が出てくるのでこれまた発行されたrootのパスワードでログイン
 っていうか http://www.kyoji-kuzunoha.com/2011/12/imoten.html これみながらやりやがれこのやろぅ！

8.ImotenはJavaSEが必要なのでこれをインストール
　これも説明だるいから、http://www.kyoji-kuzunoha.com/2012/03/oraclejavase.html　これみながらやりやがれ。

9.Imotenインストール
 mkdir /usr/local/imoten
 cd /usr/local/imoten
 wget "http://imoten.googlecode.com/files/imoten-1.1.37.zip"  ←最新版を落とすこと
 unzip -d /usr/local/imoten imoten-1.1.37.zip
 chmod 755 /usr/local/imoten/bin/imoten
 chmod 755 /usr/local/imoten/bin/wrapper*

10.Imoten設定（というか、今までのは全て「くずのは探偵事務所」のHP http://www.kyoji-kuzunoha.com/
  にやり方が書いている。「Androidでの絵文字設定除く」
　
 ここから、Android用の設定方法
 rm -f /usr/local/imoten/imoten.ini ←テンプレの設定ファイルを削除
 vi /usr/local/imoten/imoten.ini
 i打って編集モードに移行 viの使い方はググれと言いたいが

　エンターキー：カーソルを一つ下の行の先頭に移動
 k：カーソルを一つ上の行に移動
 j：カーソルを一つ下の行に移動
 l：カーソルを一つ右に移動
 h：カーソルを一つ右に移動
 x：カーソル上の文字を削除
 dd：カーソルがある行を削除
 u：直前の動作を取り消し
 i：編集モードに移行
 :w：編集を保存
 :wq：編集を保存してviエディタを終了
 :q：保存せず終了
 :q!：編集を保存せず終了
 ええ、くずのは探偵事務所さんのコピペですが何か？
 
でも、とりあえず俺のテンプレを用意しておく（これをメモしておきたかっただけ）

docomo.id=「iモード.netのID」
docomo.passwd=「iモード.netのパスワード」

smtp.server=smtp.gmail.com
smtp.port=587
smtp.connecttimeout=10
smtp.timeout=30
smtp.tls=true
smtp.from=「iモードメールアドレス」
smtp.auth.user=「Gmailのメールアドレス」
smtp.auth.passwd=「Gmailのパスワード」

forward.to=「moperaUのメールアドレス」
forward.rewriteaddress=false
forward.headertobody=false
forward.subject.charconvfile=../conv/genDocomo2google.csv

mail.encode=UTF-8
mail.contenttransferencoding=7bit
mail.emojiverticalalign=text-bottom
mail.emojisize=15px
mail.emojiverticalalignhtml=baseline
mail.emojisizehtml=14px

emojireplace.subject=false
emojireplace.body=inline

sender.smtp.port=「10000以上の任意で」
sender.smtp.user=「適当なユーザー名」←こんがらがるからmopera.netのユーザー名と同じがオススメ
sender.smtp.passwd=「適当なパスワード」←これまたこんがらがるからmopera.netの同じでいい
sender.charconvfile=../conv/genGoogle2docomo.csv
sender.docomostylesubject=true

imodenet.checkinterval=30
imodenet.logininterval=60

save.cookie=true
adressbook.csv

これをコピペして「」内を編集してesc押して:wq
次に
vi /usr/local/imoten/address.csv
して
hogehoge@docomo.ne.jp,お前の名前
を一行書いて
esc押して:wq

vi /usr/local/imoten/conf/wrapper.conf
して
wrapper.java.command=java
を
wrapper.java.command=/usr/bin/java
に書き換え
wrapper.app.parameter.2=immf.ServerMain
を追加し
esc押して:wq

ln -s /usr/local/imoten/bin/imoten /etc/init.d/imoten
/sbin/chkconfig --add imoten
/etc/init.d/imoten start


yum install yum-cron
chkconfig yum-cron on
/etc/rc.d/init.d/yum-cron start

crontab -e
iして
00 3 * * 6 /sbin/shutdown -r now (くずのはさんをパクって土曜日の午前３時に再起動するよう設定）
を書いてesc→:wq

/etc/init.d/crond restart

11.Android側の設定
iMoNiが入っていれば「必ず」アンインストールする。
標準メールでもK-9でもいいがアカウント設定は↓
メールアドレス→MoperaUのメールアドレス
パスワード→MoperaUのパスワード

受信サーバ設定
ユーザー名→MoperaUのアカウント名
パスワード→MoperaUのパスワード
受信サーバ→mail.mopera.net

送信サーバ設定
SMTPサーバ→DTIから発行されたIPアドレス
ポート→sender.smtp.portと同じ番号
ユーザー名→sender.smtp.userで設定したユーザー名
パスワード→sender.smtp.passwdで設定したパスワード

----------
設定完了

12.テスト送信

メール通知アプリを起動し起動アプリをメールアプリに設定
メールアプリでmopera.netからdocomo.ne.jp宛に絵文字付の件名と絵文字付きの本文を適当に書いて
送信し、30秒くらい待って服を脱いで待つ。
送信したメールdocomo.ne.jpからmopera.net宛で絵文字付きでメールが返ってくれば成功

とりあえずコマンドの意味を書こうと思ったが、もう朝早いので明日書く