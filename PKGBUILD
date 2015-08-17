# Maintainer: Jonas Heinrich <onny@project-insanity.org>
# Contributor: Jonas Heinrich <onny@project-insanity.org>

pkgname=roundcubemail-git
pkgver=0.1.beta2.7639.g8aee31c
pkgrel=1
pkgdesc="A PHP web-based mail client"
arch=('any')
url="http://www.roundcube.net"
license=('GPL')
depends=('php')
optdepends=('python2')
backup=('etc/webapps/roundcubemail/.htaccess'
	'etc/webapps/roundcubemail/apache.conf')
install=roundcubemail.install
options=('!strip' 'emptydirs')
conflicts=('roundcubemail')
provides=('roundcubemail')
source=('git+https://github.com/roundcube/roundcubemail.git'
	apache.conf)
md5sums=('SKIP'
         'f11b17e2a80b383cde4af963fb307541')

pkgver() {
  cd "$SRCDEST/${pkgname/-git/}"
  git describe --always | sed 's|-|.|g' | cut -f2 -d"v"
}

prepare() {
  cd ${srcdir}/roundcubemail
  sed -i \
    -e "s|RCUBE_INSTALL_PATH . 'temp.*|'/var/cache/roundcubemail';|" \
    -e "s|RCUBE_INSTALL_PATH . 'logs.*|'/var/log/roundcubemail';|" \
    config/defaults.inc.php \
    program/lib/Roundcube/rcube_config.php
}

package() {
  mkdir -p ${pkgdir}/etc/webapps/roundcubemail
  mkdir -p ${pkgdir}/usr/share/webapps
  mkdir -p ${pkgdir}/var/log
  cd ${pkgdir}/usr/share/webapps
  cp -ra ${srcdir}/roundcubemail roundcubemail
  cd roundcubemail

  mv .htaccess $pkgdir/etc/webapps/roundcubemail/
  ln -s /etc/webapps/roundcubemail/.htaccess .htaccess

  mv config $pkgdir/etc/webapps/roundcubemail/
  ln -s /etc/webapps/roundcubemail/config config

  install -dm0750 $pkgdir/var/{log,cache}/roundcubemail
  install -Dm0644 $srcdir/apache.conf $pkgdir/etc/webapps/roundcubemail/apache.conf

#  install -dm0755 $pkgdir/etc/php/conf.d/
#  cat <<EOF >$pkgdir/etc/php/conf.d/$pkgname.ini
#open_basedir = ${open_basedir}:/etc/webapps/roundcubemail:/usr/share/webapps/roundcubemail:/var/log/roundcubemail:/var/cache/roundcubemail
#EOF

  rm -rf temp logs
}
