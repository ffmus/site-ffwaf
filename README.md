
site.conf und site.mk um die Firmware für Freifunk-Knoten der Muensterland-
Community zu bauen.

Downloads finden sich unter http://images.freifunk-muensterland.net/

Siehe auch: http://gluon.readthedocs.org/en/v2014.3.1/index.html

Letzter erfolgreicher Build ging so:

git clone https://github.com/freifunk-gluon/gluon
cd gluon
git tag
git checkout tags/v2014.3.1


make GLUON_RELEASE=0.5.1-beta$(date '+%Y%m%d') GLUON_SITEDIR=~/git/freifunk-gluon/site-ffwaf GLUON_BRANCH=beta update
make GLUON_RELEASE=0.5.1-beta$(date '+%Y%m%d') GLUON_SITEDIR=~/git/freifunk-gluon/site-ffwaf GLUON_BRANCH=beta
make GLUON_RELEASE=0.5.1-beta$(date '+%Y%m%d') GLUON_SITEDIR=~/git/freifunk-gluon/site-ffwaf GLUON_BRANCH=beta manifest
contrib/sign.sh ../secret images/sysupgrade/beta.manifest

Auf dem Server bringen:

Auf dem Server:

( cd /var/www/images/beta && rm -rf * )

Dann:

( cd images && scp -r . <server>:/var/www/images/beta )

