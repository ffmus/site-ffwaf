
site.conf und site.mk um die Firmware für Freifunk-Knoten der Muensterland-
Community zu bauen.

Downloads finden sich unter http://images.freifunk-muensterland.net/

Siehe auch: http://gluon.readthedocs.org/en/v2014.3.1/index.html

Letzter erfolgreicher Build ging so:

Zunächst gloun holen:
```
git clone https://github.com/freifunk-gluon/gluon
cd gluon
git tag
git checkout tags/v2014.3.1
```

Dann bauen:
```
export GLUON_BRANCH=stable
export GLUON_SITEDIR=~/git/freifunk-gluon/site-ffwaf
export GLUON_RELEASE=2014.3.1-2

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

Auf dem Server:
```
export GLUON_BRANCH=stable
( cd /var/www/images/${GLUON_BRANCH} && rm -rf * )
```
Dann auf dem Build Host:
```
( cd images && rsync -rv --del . images.freifunk-muensterland.net:/var/www/images/${GLUON_BRANCH} )
```
