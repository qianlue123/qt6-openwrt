# qt6-openwrt
QT 6.6.1 for Openwrt 23.05 
with modbus & serialport & location
=========================

Installation instructions 
-------------------------

just change some numbers from source `karaketir16/qt5-openwrt`

1. Add feeds in feeds.conf


```
src-git libqt https://github.com/qianlue123/qt6-openwrt.git
```

2. Update & install feeds

```
./scripts/feeds update -a && ./scripts/feeds install -a
```

3. Now you can find QT libs in Libs->Qt5

Deps: libiconv, libmariaclient

NOTE
---

qtxml需要 libxml2, libxml2需要libiconv, 但是仅仅在 menuconfig 选上 libiconv-full 没有用