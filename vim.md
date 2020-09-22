# vim
## TL;DR
```sh
pythondir="/path/to/python3"

# centos7.7
% yum-builddep vim-X11

% PATH="$pythondir/bin:$PATH" \
LDFLAGS="-L$pythondir/lib -Xlinker -export-dynamic" \
./configure --prefix=/path/to/install \
--with-x \
--enable-gui=auto \
--enable-fail-if-missing \
--enable-python3interp=yes \
--with-python3-config-dir=$pythondir/lib/python3.7/config-3.7m-x86_64-linux-gnu

% make && make install
```

## 参考
https://vim-jp.org/docs/build_linux.html

## feature
以下のfeatureを有効にする

- clipboard
- clientserver
- python3

設定はmakefileをいじるか、configureでオプション指定するか

https://vim-jp.org/vimdoc-ja/various.html#+feature-list

確認するには

```sh
% vim --version
```

## configure
### python3
事前にpython3をインストール  
日本語マニュアルには、事前に`python3-dev`のインストールが必要とあるが、ソースからpythonをインストールするので気にしなくてよいと思われ

- PATH="$pythondir/bin:$PATH"  
configureはpathからpythonを見つける。使いたいpythonが前にくるように

- LDFLAGS="-L$pythondir/lib  
必要かどうか不明

- -Xlinker -export-dynamic"  
以下の import \_posixsubprocess エラーを参照

- --enable-fail-missing  
エラーチェック

- --enable-python3interp=yes  
python3を有効に。`yes`/`dynamic` の違いは未調査

python2と3を両方有効にする場合は以下に気になる説明があるので参照すること

https://vim-jp.org/vimdoc-ja/if_pyth.html

- --with-python3-config-dir=/path/...  
使いたいpythonに応じて

#### import \_posixsubprocess エラー
ソースからbuildしたpythonを指定すると、buildはできたが、`python-mode.vim`や`jedi-vim`を起動した際に原因不明のエラー

```
The error was: /path/to/python/lib-dynload/_posixsubprocess.cpython-36m-x86_64-linux-gnu.so: undefined symbol: PyTuple_Type
```

`import _posixsubprocess`している箇所で上記エラー

以下を実行すると再現

```
:py3 import _posixsubprocess
```

vimのビルド時に`LDFLAGS=-Xlinker -export-dynamic`を指定することで解決  
別件でcにpythonを組み込むことをやった時のコンパイルオプションを参考にした。以下

`/path/to/python/bin/python3.7-config --ldflags`

### clipboard, clientserver
readmeからはどのように指定すればよいか読み取れなかった

参考  
http://d.hatena.ne.jp/tashen/20090606/1244294383  
http://blog.bonar.jp/entry/20090308/1236527086

configureのオプションを以下のように設定

```sh
% ./configure --with-x --enable-gui=auto
```

X11のヘッダ等が必要なため、事前にインストールしておく

```sh
# centos7
% yum-builddep vim-X11
```
