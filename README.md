
site.conf und site.mk um die Firmware für Freifunk-Knoten der Warendorfer
Community zu bauen.

Downloads finden sich unter http://images.freifunk-muensterland.net/

Siehe auch: http://gluon.readthedocs.org/en/v2014.4/index.html

Letzter erfolgreicher Build ging so:

Zunächst gloun holen:
```
git clone https://github.com/freifunk-gluon/gluon gluon-stable
cd gluon-stable
git checkout tags/v2014.4
```

Dann bauen:
```
export GLUON_BRANCH=stable
export GLUON_SITEDIR=~/git/freifunk/site-ffwaf

make update
make clean
make -j5
```

Jetzt noch die Images veröffentlichen:
```
make manifest
contrib/sign.sh ../secret images/sysupgrade/${GLUON_BRANCH}.manifest
```

Auf dem Server bringen:

```
( cd images && rsync -rv --del . images.freifunk-muensterland.net:/var/www/images/${GLUON_BRANCH} )
```
