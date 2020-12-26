# Checkmate engine patch using GPSshogi/OpenShogiLib

このリポジトリでは[GPSshogi](https://gps.tanaka.ecc.u-tokyo.ac.jp/gpsshogi/)を使った詰将棋ソルバの導入方法と各種ツールを提供します。GPS将棋の詰将棋ソルバはDepth first proof number searchをベースとしており図巧を始めとした難解な詰将棋を高速に解くことができます。

This reposiitory provides installation guide and utilities for Tsume-Shogi (Checkmate problem) solver using [GPSshogi](https://gps.tanaka.ecc.u-tokyo.ac.jp/gpsshogi/). Tsume-Shogi solver in GPSshogi uses Depth first proof number search and it can solve difficult problems (like Shogi-Zuko) quickly.

# How to install

## step1
GPSShogiのサブライブラリであるOpenShogiLib(osl)をダウンロードします。[oslのダウンロードページはこちら](https://gps.tanaka.ecc.u-tokyo.ac.jp/gpsshogi/index.php?%BB%C8%A4%A4%CA%FD)、[直リンクはこちら](http://gps.tanaka.ecc.u-tokyo.ac.jp/cgi-bin/viewvc.cgi/trunk/osl.tar.gz?view=tar)

Download OpenShogiLib(osl) from [here(download page)](https://gps.tanaka.ecc.u-tokyo.ac.jp/gpsshogi/index.php?%BB%C8%A4%A4%CA%FD) or [here(direct link)](http://gps.tanaka.ecc.u-tokyo.ac.jp/cgi-bin/viewvc.cgi/trunk/osl.tar.gz?view=tar)

## step2

boostをインス トールします(メッチャ面倒)

Unfortunately, you have to install boost (sometimes, it will be a painful task)

### Ubuntu

`sudo apt-get install libboost-all-dev`

### Ubuntu以外

ask google or use Ubuntu

## step.3 Compile

### fix configure

oslのライブラリは非常に古いため最新のgccの規約に反したコードが普通に存在しています。コンパイルを通すには`-fpermissive`オプションが必要です。

You need to add `-fpermissive` option because osl library violate current coding rule in gcc.


`osl-osl/osl/makefile.conf`の

Please replace

    RELEASE_CXX_OPTFLAGS = -O3 -DNDEBUG $(CPUOPTION)
を
    
	RELEASE_CXX_OPTFLAGS = -O3 -DNDEBUG $(CPUOPTION) -g -fpermissive

に書き換える必要があります。

like above.

### Add Principal Variation[Optional]

詰み手順を表示するためのルーチンを追加できます。`osl-osl/osl/sample/checkmate.dfpnstat.cc`を本リポジトリの`src/dfpnstat.cc`で上書きすることで動作します。

You can show principal variation by replacing `osl-osl/osl/sample/checkmate.dfpnstat.cc` to `src/dfpnstat.cc`(in this repository).

### compile


    cd osl-osl/osl/sample
	make


### test

以下のコマンドで詰将棋を解くことが可能です。

You can solve Tsume-Shogi by following command.

    cd checkmate
	./dfpnstat -d -l nodelimit problem_to_solve.csa

`-d`はログを表示するオプション。`-l`は探索するノードの数、`problem_to_solve.csa`は扱いたい問題を`csa`形式にしたものです。

`-d` is a flag to show debugging log, `-l` is an option to fix node limit and `problem_to_solve.csa` is a file name of initial position by csa format.

一例としてexampleの図巧の問題を解くと以下のようなログが得られます

For example, you can solve Zuko problem using example in this repository.

    ./dfpnstat -d -l 11451419 examples/zuko03.csa 


    maxdepth46
    Drop(+,SILVER,Square(77)) Move(-,KING,Square(86)->Square(95)) Drop(+,GOLD,Square(96)) Move(-,KING,Square(95)->Square(94),capture=PAWN) Drop(+,KNIGHT,Square(86)) Move(-,ROOK,Square(89)->Square(86),capture=KNIGHT) Move(+,GOLD,Square(96)->Square(85),capture=PAWN) Move(-,ROOK,Square(86)->Square(85),capture=GOLD) Drop(+,PAWN,Square(95)) Move(-,KING,Square(94)->Square(95),capture=PAWN) Drop(+,PAWN,Square(96)) Move(-,KING,Square(95)->Square(94)) Drop(+,KNIGHT,Square(86)) Move(-,ROOK,Square(85)->Square(86),capture=KNIGHT) Move(+,PAWN,Square(96)->Square(95)) Move(-,KING,Square(94)->Square(95),capture=PAWN) Move(+,SILVER,Square(77)->Square(86),capture=ROOK) Move(-,KING,Square(95)->Square(86),capture=SILVER) Drop(+,ROOK,Square(83)) Drop(-,LANCE,Square(84)) Move(+,PROOK,Square(83)->Square(84),promote,capture=LANCE) Move(-,KING,Square(86)->Square(76),capture=KNIGHT) Move(+,PROOK,Square(84)->Square(85)) Move(-,KING,Square(76)->Square(66)) Move(+,PROOK,Square(85)->Square(55)) Move(-,KING,Square(66)->Square(55),capture=PROOK) Move(+,PBISHOP,Square(78)->Square(56)) Move(-,KING,Square(55)->Square(64)) Move(+,PBISHOP,Square(56)->Square(65)) Move(-,KING,Square(64)->Square(73),capture=PAWN) Move(+,PBISHOP,Square(74)->Square(83),promote) Move(-,KING,Square(73)->Square(63)) Move(+,PBISHOP,Square(65)->Square(74)) Move(-,KING,Square(63)->Square(54),capture=PAWN) Move(+,PBISHOP,Square(74)->Square(65)) Move(-,KING,Square(54)->Square(63)) Move(+,PBISHOP,Square(83)->Square(74)) Move(-,KING,Square(63)->Square(72)) Drop(+,PAWN,Square(73)) Move(-,KING,Square(72)->Square(61)) Move(+,PBISHOP,Square(65)->Square(43)) Move(-,PAWN,Square(42)->Square(43),capture=PBISHOP) Move(+,PPAWN,Square(41)->Square(51),capture=GOLD) Move(-,KING,Square(61)->Square(51),capture=PPAWN) Move(+,PBISHOP,Square(74)->Square(41)) Move(-,KING,Square(51)->Square(41),capture=PBISHOP)


## Lisence

OpenShogiLibraryはbsd風のライセンスになっています。詳しくはLISENCEを参照してください。rsawadaが作成したマニュアル、及びsrc以下のファイルについてはMITライセンスです。

Please see LISENCE for OpenShogiLibrary. License of download manual and attachment (src/*) is written by rsawada is MIT License.

## todo

- bugfix in pv (何故か最後の一手が出力されない)
