<!--

This document is written in Markdown.
You can preview on such as VisualStudio Code.
If you want to know more, search with "vscode markdown" or refer to official document https://code.visualstudio.com/Docs/languages/markdown .

-->

# WSL2 セットアップ

`Windows Subsystem for Linux`、通称`WSL`はWindows上でLinuxの実行ファイルを実行するための機能である。

ここでは`WSL`について説明した上で、そのバージョン2、`WSL2`の環境構築の手順について解説する。

## WSLについて

従来WindowsではコマンドプロンプトやPowerShellなどのCUI操作が可能である一方で、UNIX系のコマンドやオープンソース開発のためのLinuxとの互換が期待されていた。

そこでWindows10では当初、Windowsのkernel上にLinuxライクなkernelで構成されていた`WSL`の"バージョン1"、`WSL1`が搭載された。

しかし、この`WSL1`ではWindows kernel上に載っていることによる様々なボトルネック、そしてそもそもLinux kernelそのものでないことによる互換性の問題があった。

そしてWindows(Windows10 ver.2004以降)には`WSL2`がリリースされた。

`WSL2`では下記の図右のようにWindowsそのものもHyper-Vという仮想化技術で仮想化しLinux kernelそのものを搭載することができるようになった。

これによりWindowsにおけるLinux開発の敷居は下がり、容易にLinuxコマンドも実行することが可能となっている。

またWindows11から(厳密には10のInsider Preview)は`WSLg`というWindows上にサブシステムのLinuxからのウィンドウを表示することができるようになった。従来もWindows側に`X Window`サーバーを立てれば可能ではあったがそれを正式にサポートするようになった形である。

ただし、もし`WSL`を有効化するならば、当然通常よりメモリを消費すること、下の図のようにWindows自身も仮想化することによりWindowsのパフォーマンスも(感じるほどではないが)やや低下するということを頭に入れておきたい。

メモリは8GBでも通常のWindowsの使用には問題がないが限界に近いので16GBは目安として搭載しているほうが良い。

![](./docs/image/wsl.png)


## サブシステムのLinuxディストリビューションの概要

サブシステムとして利用可能なLinuxディストリビューションは管理者モードのPowerShellのコマンドからもダウンロード&インストール可能であるが普通は`Microsoft Store`から入れることが一般的だろう。`Microsoft Store`でそのディストロの名前を検索すれば出てくる。

入れることができるディストリビューションの一例
- Ubuntu
- Debian
- openSUSE

ちなみに`WSL`有効化前に入れると起動にコケたはずなので先に入れないように気を付ける。

## WSLの有効化・Linuxの起動

詳しい説明は公式ドキュメントを参照していただきたい。

[WSL の基本的なコマンド](https://docs.microsoft.com/ja-jp/windows/wsl/basic-commands)


1. インストール

	やり方は2通り

	1. コントロールパネルから

		`コントロールパネル > プログラム > プログラムと機能 > Windowsの機能の有効化または無効化` から

		`Linux用Windowsサブシステム`にチェックを入れて有効化し再起動。

		![](./docs/image/WSLEnabling.png)

	2. PowerShellから

		PowerShell/Windows Terminalを"管理者として"実行

		```PowerShell
		wsl --install
		```

2. WSLのデフォルトバージョンを2に

	PowerShell/Windows Terminalを管理者として実行。以下のコマンドを実行

	```PowerShell
	wsl --set-default-version 2
	```

	もし変更を忘れても後から変換は可能。詳しくは公式ドキュメントで。

3. ディストリビューションのインストール

	`Microsoft Store`からUbuntuなどを検索してダウンロード&インストール

4. WSLのLinux起動

	`Microsoft Store`からダウンロードしたのでスタートメニューにアイコンが表示されるからそのアイコンをクリックして起動。

	もしくは

	`Windows Terminal`のタブの下矢印ボタンから追加したディストロを選択することで起動できる。

## WSLのディレクトリの立ち位置

Windows側のディレクトリは`WSL`から見ると"Cドライブ"の場合は"/mnt/c"にマウントされる形になっている。しかしWSLのホームディレクトリ"~"は"/home/\_LINUX\_USER\_NAME\_"なので気を付けていただきたい。

例えばWindowsのデスクトップ上で右クリックからsource.cを作成すれば

WSL Ubuntuから`/mnt/c/Users/_WIN_USER_NAME_/Desktop/`を`cd`や`ls`で参照できる。
```
cd /mnt/c/Users/_WIN_USER_NAME_/Desktop/
```

同様に、

```
gcc source.c
```
とすれば通常のUbuntu通り、`gcc`が実行できる。当然、`gcc`は`apt`でインストールする必要はある。

## WSLgの有効化

説明は公式ドキュメントを参照していただきたい。
[Linux 用 Windows サブシステム (プレビュー) で Linux GUI アプリを実行する](https://docs.microsoft.com/ja-jp/windows/wsl/tutorials/gui-apps)


<!-- Written by Croyfet in 2022-->
