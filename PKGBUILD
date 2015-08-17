# Maintainer: Alastair Stuart <alastair@muto.so>

pkgname=synergy-svn
pkgver=1880
pkgrel=1
pkgdesc="Share a single mouse and keyboard between multiple computers"
url="http://synergy-foss.org"
arch=('i686' 'x86_64')
depends=('gcc-libs' 'libxtst' 'libxinerama' 'bash')
conflicts=('synergy' 'synergy-beta')
provides=('synergy' 'synergy-beta')
license=('GPL2')
makedepends=('libxt' 'python2' 'svn' 'cmake')       # used by configure to test for libx11...

_svntrunk="http://svn.synergy-foss.org/trunk/"
_svnmod="synergy"

build() {
  cd $srcdir
  msg "Connecting to $_svntrunk SVN server...."

  if [ -d $_svnmod/.svn ]; then
    (cd $_svnmod && svn up -r $pkgver)
    msg "SVN update done or server timeout"
  else
    svn co $_svntrunk --config-dir ./ -r $pkgver $_svnmod
    msg "SVN checkout done or server timeout"
  fi
  
  cp -r $_svnmod $_svnmod-build
  cd $_svnmod-build

  python2 hm.py conf -g1
  python2 hm.py build
}

package() {
  msg "Packaging in $pkgdir"
  cd $srcdir/$_svnmod-build
  
  # install binary
  install -d "$pkgdir/usr/bin/"
  install -Dm755 bin/synergyc "$pkgdir/usr/bin/"
  install -Dm755 bin/synergys "$pkgdir/usr/bin/"
  install -d "$pkgdir/etc/"
  install -Dm644 doc/synergy.conf.example "$pkgdir/etc/"
}
