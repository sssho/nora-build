# emacs
emacs 26.3

## TL;DR
```sh
% LIBGNUTLS_CFLAGS=-I/path/to/install/include \
LIBGNUTLS_LIBS="-L/path/to/install/lib -Wl,-rpath,/path/to/install/lib -lgnutls" \
./configure --prefix=/path/to/install --with-jpeg=no --with-gif=no --with-tiff=no

% make && make install
```

## Dependencies(Optional)
- gnutls 3.6.14
- gmp 6.2.0
- nettle 3.6

emacsでwebアクセスが発生する処理(パッケージインストール等)をする予定がある場合は、`gnutls`のbuildが必要(依存である`gmp`、`nettle`も)。そうでない場合は、emacsのconfigure時に`--with-gnutls=no`を指定

## gmp
https://gmplib.org/

```sh
% ./configure --prefix=/path/to/install
% make && make install
```

## nettle
https://ftp.gnu.org/gnu/nettle/

`gmp`を参照させないと、`libhogweed`がbuildされないので注意。`libhogweed`は`gnutls`のbuildに必要

```sh
# CFLAGS、LDFLAGSはgmpのインストール先を指定
% CFLAGS=-I/path/to/install/include \
LDFLAGS="-L/path/to/install/lib -Wl,-rpath,/path/to/install/lib -lgmp" \
./configure --prefix=/path/to/install

% make && make install
```

## gnutls
https://www.gnutls.org/

`GMP_LIBS`はrpath指定しなくてもリンクできた。`lhogweed`が`gmp`をリンクしているから？

```sh
% GMP_CFLAGS=-I/path/to/install/include \
GMP_LIBS=-L/path/to/install/lib \
HOGWEED_CFLAGS=-I/path/to/install/include \
HOGWEED_LIBS="-L/path/to/install/lib64 -Wl,-rpath,/path/to/install/lib64 -lhogweed" \
NETTLE_CFLAGS=-I/path/to/install/include \
NETTLE_LIBS="-L/path/to/install/lib64 -Wl,-rpath,/path/to/install/lib64 -lnettle" \
./configure --prefix=/path/to/install \
--with-included-libtasn1 \
--with-included-unistring \
--without-p11-kit \
--disable-doc

% make && make install
```

>  rpathの書き方ではまった
> `-rpath "$INSTALL_PATH"/lib`  
> のように、スペースで区切るとただしくrpathが設定されない
> readelfでバイナリを確認すると

```sh
% readelf -w -d libgnutls.so

...
0x00..00f (RPATH)   Library path: [-rpath]
...
```

## emacs
`--with-jpeg==no`、`--with-gif==no`、`--with-tiff=no`はライブラリが入っていれば除外不要

```sh
% LIBGNUTLS_CFLAGS=-I/path/to/install/include \
LIBGNUTLS_LIBS="-L/path/to/install/lib -Wl,-rpath,/path/to/install/lib -lgnutls" \
./configure --prefix=/path/to/install --with-jpeg=no --with-gif=no --with-tiff=no

% make && make install
```
