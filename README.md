
site.conf und site.mk um die Firmware für Freifunk-Knoten der Muensterland-
Community zu bauen.

Downloads finden sich unter http://images.freifunk-muensterland.net/

Siehe auch: http://gluon.readthedocs.org/en/v2014.3.1/index.html

Letzter erfolgreicher Build ging so:

git clone https://github.com/freifunk-gluon/gluon
cd gluon
git tag
git checkout tags/v2014.3.1


make GLUON_RELEASE=0.5.1-exp$(date '+%Y%m%d') GLUON_SITEDIR=~/git/freifunk-gluon/site-ffwaf GLUON_BRANCH=experimental update
make GLUON_RELEASE=0.5.1-exp$(date '+%Y%m%d') GLUON_SITEDIR=~/git/freifunk-gluon/site-ffwaf GLUON_BRANCH=experimental
make manifest GLUON_RELEASE=0.5.1-exp$(date '+%Y%m%d') GLUON_SITEDIR=~/git/freifunk-gluon/site-ffwaf GLUON_BRANCH=experimental
contrib/sign.sh ../secret images/sysupgrade/experimental.manifest

Auf dem Server bringen:

Auf dem Server:

( cd /var/www/images/experimental && rm -rf * )

Dann:

( cd images && scp -r . <server>:/var/www/images/experimental 

