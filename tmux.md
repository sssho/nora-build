# tmux
## TL;DR
- Install dependencies (ncurses, livevent)
 
```sh
% PKG_CONFIG_PATH="/path/to/install/lib/pkgconfig" \
LIBEVENT_LIBS="-L$/path/to/install/lib -Wl,-rpath,/path/to/install/lib -levent" \
./configure --prefix=/path/to/install > /dev/null

% make && make install
```

## Dependencies
1. ncurses
1. livevent

## ncurses
- configure内部でegrepが使われるため、**grepの色設定をしているとエラー**
- pkg-config用のファイルを出力して、後で使う

```sh
GREP_OPTIONS= ./configure --prefix=/path/to/install
--enable-pc-files --with-pkg-config-libdir=/path/to/install/lib/pkgconfig --with-termlib
```

## libevent
- pkgconfigファイルはデフォルトで生成される

## tmux
- ncurses/livevent は PKG_CONFIG_PATH で指定
- prefixのみの指定だと、makeは通るが **leventがnot found** になる

```sh
% git clone repo 
% cd tmux
# configure生成
% ./autogen.sh
```

```sh
% PKG_CONFIG_PATH="/path/to/install/lib/pkgconfig" \
LIBEVENT_LIBS="-L$/path/to/install/lib -Wl,-rpath,/path/to/install/lib -levent" \
./configure --prefix=/path/to/install > /dev/null

% make && make install
```
