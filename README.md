
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
git checkout df018ed7c7f33006565918bf7d4cc4caab631aee
```

Dann bauen:
```
export GLUON_BRANCH=latest
export GLUON_SITEDIR=~/git/freifunk/ffmus/site-ffwaf

make update
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
```
