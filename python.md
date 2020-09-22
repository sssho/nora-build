# python
## TL;DR
```sh
$ CFLAGS=-I/path/to/insall/include LDFLAGS="-Wl,-rpath,/path/to/install/lib64 -L/path/to/install/lib64 -lffi" ./configure --prefix=/path/to/install
$ make && make install
```

## libffi
3.8 (3.7も?)から`_ctypes`のビルドが失敗するように

```
The necessary bits to build these optional modules were not found:
_bz2                  _lzma                 _sqlite3
_tkinter
To find the necessary bits, look in setup.py in detect_modules() for the module's name.


The following modules found by detect_modules() in setup.py, have been
built by the Makefile instead, as configured by the Setup files:
_abc                  atexit                pwd
time


Failed to build these modules:
_ctypes
```

解決策として、依存パッケージである`libffi`をインストール

以下、ローカルに`libffi`をインストールする方法

```sh
$ git clone https://github.com/libffi/libffi.git -b v3.3 libffi-v3.3
$ cd libffi-v3.3
$ ./autogen.sh
$ ./configure --prefix=/path/to/install --disable-docs
$ make
& make check
& make install
```

## python
`CFLAGS`、`LDFLAGS`を指定してconfigure、build

```sh
$ CFLAGS=-I/path/to/insall/include LDFLAGS="-Wl,-rpath,/path/to/install/lib64 -L/path/to/install/lib64 -lffi" ./configure --prefix=/path/to/install
$ make && make install
```
