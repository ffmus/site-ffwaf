
site.conf und site.mk um die Firmware für Freifunk-Knoten der Warendorfer
Community, alias Domäne-14 von Freifunk-Münsterland, zu bauen.

Downloads finden sich unter http://images.freifunk-muensterland.net/

Siehe auch: http://gluon.readthedocs.org/en/v2016.1/

Letzter erfolgreicher Build ging so:

Zunächst gloun holen:
```
git clone https://github.com/freifunk-gluon/gluon gluon-stable
cd gluon-stable
git checkout master
git pull
git checkout tags/v2016.1
```

Dann bauen:
```
export GLUON_BRANCH=stable
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
