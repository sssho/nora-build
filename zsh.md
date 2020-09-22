# zsh
## TL;DR
```sh
% ./configure --prefix=/path/to/install --sysconfdir=/path/to/install/etc/zsh --enable-etcdir=/path/to/install/etc/zsh
% make && make install
```

## configure
configureに`--sysconfdir`、`--enable-etcdir`を指定することで、`/etc`以下のシステムファイルを読まないようにする  
指定したディレクトリ以下にシステムファイルがなくてもエラーにはならない

両方指定するのが本当に必要かは調査していない
