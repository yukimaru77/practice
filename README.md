# 初めてcloneした時

## ワークスペースの開き方
cloneすると"practice"フォルダの中に"`マークダウン.code-workspace`"があるので、vsを開き"ファイル"→"ファイルでワークスペースを開く"で先ほどのファイルを選択する

## md→pdf、htmlへの変換設定
1. vsの拡張機能でMarkdown PDFをインストールする。
2. コマンドパレットで ```> Extensions: Open Extensions Folder```と入力する。
3. ```yzane.markdown-pdf```から始まるフォルダを開き、その中のtemplate→template.htmlを開く。
4. そこに"practice"フォルダのtemplate.htmlをコピーして保存する。

## md→pdf、htmlへの変換方法
1. mdファイルを開いて右クリックする。
2. 変換(Export)をクリックする。

## 行列式について
行列式は\\\\が文中に必要になるがhtmlはこれをエスケープした\\と見てしまうため\\\\は\crと書くことでこれを回避できる。