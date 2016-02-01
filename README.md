
site.conf und site.mk um die Firmware für Freifunk-Knoten der Warendorfer
Community zu bauen. Dieser Zweig ist höchst experimentel. Die Chancen das
etwas nicht funktioniert, oder das etwas kaputt geht, sind recht hoch.
Daher die ausdrückliche und dringende Empfehlung diesen Zweig *NICHT* zu
nutzen.

Downloads finden sich unter http://images.freifunk-muensterland.net/

Siehe auch: http://gluon.readthedocs.org/en/latest/

Letzter erfolgreicher Build ging so:

Zunächst gloun holen:
```
git clone https://github.com/freifunk-gluon/gluon gluon-latest
cd gluon-latest
git checkout master
git pull
git checkout 54ed2a8d1407655f3ddeac5e0dd6c9e8b4d402fd
```

Dann bauen:
```
export GLUON_BRANCH=latest
export GLUON_SITEDIR=~/git/freifunk/ffmus/site-ffwaf

make update
make GLUON_TARGET=ar71xx-generic
```

Nach Änderungen im site.mk/site.conf etc. ist dann jeweils folgendes
auszuführen:

```
make clean GLUON_TARGET=ar71xx-generic
make -j5 GLUON_TARGET=ar71xx-generic
```

Jetzt noch die Images veröffentlichen:
```
make manifest
contrib/sign.sh ../secret output/images/sysupgrade/${GLUON_BRANCH}.manifest
```

Auf dem Server bringen:

```
( cd output/images && rsync -rav --del . images.freifunk-muensterland.net:/var/www/images/${GLUON_BRANCH} )
( cd output/modules && rsync -rav . images.freifunk-muensterland.net:/var/www/images/modules )
```

Für einen lokalen mirror (openwrt bietet das nicht in ipv6 an) etwa folgendes
ausführen (Muss man nur einmal machen):

```
wget -N -r -l inf -p -np -k https://downloads.openwrt.org/chaos_calmer/15.05/ar71xx/generic/packages/
```
