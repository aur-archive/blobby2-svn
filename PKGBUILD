#Contributor: Urs Wolfer <uwolfer @ fwo.ch>

pkgname=blobby2-svn
_realname=blobby2
pkgver=1259
pkgrel=1
pkgdesc="Blobby Volley is one of the most popular freeware (now open source!) games."
#url="http://blobby.sourceforge.net/"
url="http://sourceforge.net/projects/blobby/"
license=('GPL')
conflicts=('blobby2')
depends=('sdl' 'unzip' 'physfs')
makedepends=('subversion' 'cmake')
arch=(i686 x86_64)
source=($_realname.desktop
	$_realname.png)

md5sums=('ab05bed794ee78db693fd3036393275a'
         'c1bc427b41a0a3facd771ac83c7fb412')

_svntrunk=svn://svn.code.sf.net/p/blobby/code/trunk
_svnmod=blobby2

build() {
  cd "$srcdir"
  if [ -d $_svnmod/.svn ]; then
    (cd $_svnmod && svn revert * -R && svn up -r $pkgver)
  else
    svn co $_svntrunk --config-dir ./ -r $pkgver $_svnmod
  fi
  #patch -p0 < $srcdir/install.patch
  msg "SVN checkout done or server timeout"
  msg "Starting make..."

  # Build
  #rm -rf ${srcdir}/build
  mkdir -p ${srcdir}/build
  cd ${srcdir}/build || return 1
  cmake -DCMAKE_INSTALL_PREFIX=/usr ../${_svnmod}
  make || return 1
  make DESTDIR=$pkgdir install || return 1

  # install .desktop file and icon
  install -dm755 $pkgdir/usr/share/{applications,pixmaps}
  install -m644 $srcdir/$_realname.desktop $pkgdir/usr/share/applications
  install -m644 $srcdir/$_realname.png $pkgdir/usr/share/pixmaps
}
