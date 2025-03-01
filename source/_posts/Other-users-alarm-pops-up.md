---
title: 他人の予定のアラームが表示された際の確認ポイント
date: 2025-02-27
lastupdate:
tags: Outlook
---
 
こんにちは。日本マイクロソフト Exchange & Outlook サポート チームの御正 (ミショウ) です。

本記事では Outlook にて他人の予定アイテムのアラームが表示される事象が発生したときのチェック ポイントについて説明します。
なお、本件では従来の Outlook for Windows (以下、従来の Outlook) に焦点を当てて記載しています。
 
## 予定アイテムのアラームについて
Outlook を利用している際、起動直後や設定した予定や会議の開始前にアラーム通知が表示されます。

![](alarm.png)

このアラームが表示された際、表示されている予定/会議アイテムをダブル クリックし、[ファイル] を選択すると、アイテムが格納されているフォルダー情報を確認することが可能です。

![](currentfolder.png)

稀に自分が設定した覚えがなく、自身の予定表にも登録されていない予定/会議のアラームが表示される場合があります。その際に確認するポイントについて本記事で紹介します。

## A. 削除済みアイテム フォルダー内にある予定表の確認
メールボックスの [削除済みアイテム] フォルダーに、過去に削除した予定表フォルダーは残っていないでしょうか？
[削除済みアイテム] に移動された予定表フォルダーが完全に削除されたわけではないので、そこにあるアイテムのアラームが表示される可能性があります。

1. Outlook の左パネルにある [その他のアプリ] を選択し、[フォルダー] を選択します。

      ![](folder.png)

2. [削除済みアイテム] を展開し、対象の予定表フォルダーを右クリックして [予定表の削除] を選択し、完全削除します。

次にオンライン アーカイブ メールボックスでも同様に確認してみましょう。
 
1. Outlook の左パネルにある [その他のアプリ] を選択し、[フォルダー] を選択します。
2. Outlook の左パネルからオンライン アーカイブを展開します。
3. [削除済みアイテム] を展開し、対象の予定表フォルダーを右クリックして [予定表の削除] を選択し、完全削除します。

## B. PST ファイルの確認
今利用している Outlook で PST ファイルは開いていますか？開いている場合、当該 PST ファイルを右クリックして、[データ ファイルのプロパティ] を選択し、[このフォルダーのアラームとタスクを To Do バーに表示する] がオンになっているかを確認しましょう。  
オンになっている場合、オフにし、[適用] をクリックした後、Outlook 再起動のメッセージが表示されます。  
複数の PST を開いている場合、それぞれの [このフォルダーのアラームとタスクを To Do バーに表示する] 設定を確認し、変更したうえで、Outlook を再起動します。

## C. MFCMAPI の利用
ここまで来たら、もしかしたら他人の予定表フォルダーが隠しアイテムとしてメールボックスに残っているかもしれません。もしくは、ユーザーが利用しているオンライン アーカイブに他人の予定表フォルダーが保存されている可能性もあります。
少々複雑な手順ですが、MFCMAPI といったツールを利用して、アラームの削除と、予定表フォルダーの削除をしてみます。

1. 事前に Outlook にて追加している予定表 (アラームが表示されてしまう予定表のみで構いません) を削除します。
2. MFCMAPI をダウンロードします。

- [GitHub](https://github.com/microsoft/mfcmapi/releases/tag/24.0.24362.01) から事前に MFCMAPI のファイルをダウンロードして展開し、展開された MFCMapi.exe のプロパティで [許可する] をオンにしておきます。

    32 ビット版の Outlook を使用している場合: MFCMAPI.exe.<バージョン>.zip  
    64 ビット版の Outlook を使用している場合: MFCMAPI.x64.exe.<バージョン>.zip

    ※ 使用している Outlook が 32 ビット版か 64 ビット版かは、[ファイル] タブ-[Office アカウント]-[Outlook のバージョン情報] から確認します。 

3. Outlook を終了します。
4. MFCMAPI.exe をダブルクリックして起動します。
5. [Tools] メニューの [Options] をクリックし、以下のチェックボックスをオンになっていない場合はオンにして [OK] をクリックします。

    Use the MDB_ONLINE_ flag when calling OpenMsgStore  
    Use the MAPI_NO_CACHE flag when calling OpenEntry

6. [Session]、[Logon] の順にクリックし、Outlook プロファイルを選択して、[OK] をクリックします。
7. 対象のメールボックス ストアをダブルクリックします。
8. 画面中央に開かれた画面左側のツリー [Root Container] をクリックして展開させます。
9. 展開表示された左メニューを下までスクロールし、[アラーム] フォルダーの上で右クリック、[Delete folder] をクリックします。
10. 確認ダイアログが表示されますので [OK] をクリックします。
11. [Root Container] - [Top of Information Store (インフォメーション ストアの先頭)] - [Calendar (予定表)] を展開します。
12. [Calendar (予定表)] フォルダー配下に、アラームが表示される予定表の持ち主であるユーザー名を含むフォルダーを探します。(複数存在している場合もございます。)
13. 手順 9. で確認したフォルダーを右クリックして [Delete folder] をクリックし、[Hard Deletion] をオンにして [OK] をクリックします。
14. 再度 [Calendar (予定表)] フォルダーを展開し、手順 10. で削除したフォルダーが存在しないことを確認します。
15. 同様の手順を繰り返し、[Calendar (予定表)] フォルダー配下にアラームが表示される予定表フォルダーがなくなるまでフォルダーを削除します。
16. ウィンドウを閉じ 7 番の手順を今度はオンライン アーカイブ メールボックスで行います。
17. 11 から 15 の手順をオンライン アーカイブ メールボックスでも繰り返し行います。
18. Outlook を起動し、事象の発生状況を確認します。
 
## D. cleanreminders スイッチの実行
Outlook には cleanreminders スイッチがあり、アラームの削除と再生成ができます。  
以下のスイッチを付けて Outlook を起動し、事象が直るか確認してみましょう。

1. Outlook を終了します。
2. Windows の画面左下のスタートメニューから、[ファイル名を指定して実行] を起動します。
3. [名前:] 横のダイアログに以下のように入力し Enter を押下します。
   ※ Outlook.exe と " / " の間には半角スペースが必要となりますのでご注意ください。

       Outlook.exe /cleanreminders

4. Outlook が起動します。

**注意**  
/cleanreminders を実施すると、内部的にアラーム フォルダーを削除、再作成を行い、その後アラーム情報を再度検索し、取得するような動作が行われます。
そのため、検索対象のアイテムが多い場合や、取得するアラーム情報が多い場合には、アラームが正常に表示されるまでに時間を要することが予測されます。

## E. OST ファイルの再作成
Outlook on the Web にて他人の予定アイテムのアラームが表示されず、Outlook のみ表示されてしまう場合、ローカルのキャッシュ ファイルにサーバーとは同期されていない予定表が残っているかもしれません。  
OST ファイルを削除し、問題が解決されるか見てみましょう。
 
1. Outlook を起動し [ファイル]-[アカウント設定]-[アカウント設定] を開きます。
2. [データ ファイル] タブを開き、自身のアカウントを選択し [ファイルの場所を開く] を選択してエクスプローラーを開きます。これで OST ファイルの場所を特定します。
3. Outlook を終了し、先程特定した OST ファイルを削除します。

**本情報の内容（添付文書、リンク先などを含む）は、作成日時点でのものであり、予告なく変更される場合があります。**
