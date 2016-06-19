# Contributor: David Keogh <davekeogh@archlinux.us>

pkgname=pycharm-community
pkgver=2016.1.4
pkgrel=3
pkgdesc="Powerful Python and Django IDE. Community edition."
arch=('any')
options=('!strip')
url="http://www.jetbrains.com/pycharm/"
license=('Apache')
depends=('giflib' 'ttf-font')
makedepends=('python2-setuptools' 'python-setuptools')
conflicts=('pycharm' 'pycharm-professional')
provides=('pycharm')
source=(https://download.jetbrains.com/python/$pkgname-$pkgver.tar.gz
        'pycharm-community.desktop' )
sha256sums=('bc548a200022111e741604e76b579e71e5a75d23c2bbb9c4cb9ed88b18a3d622'
            '5ce44b5bee632258749ee7d0df0fd08af446d43715f9ef50cb6889b88232de41')

package() {
  # compile PyDev debugger used by PyCharm to speedup debugging
  python2 $srcdir/$pkgname-$pkgver/helpers/pydev/setup_cython.py build_ext --inplace &
  python3 $srcdir/$pkgname-$pkgver/helpers/pydev/setup_cython.py build_ext --inplace &
  for job in `jobs -p`
  do
      wait $job
  done

  cd $srcdir
  mkdir -p $pkgdir/opt/$pkgname
  cp -R $srcdir/$pkgname-$pkgver/* $pkgdir/opt/$pkgname

  if [[ $CARCH = 'i686' ]]; then
    rm -f $pkgdir/opt/$pkgname/bin/libyjpagent-linux64.so
    rm -f $pkgdir/opt/$pkgname/bin/fsnotifier64
  fi

  mkdir -p $pkgdir/usr/share/{applications,pixmaps}
  install -Dm644 $startdir/pycharm-community.desktop $pkgdir/usr/share/applications/
  install -Dm644 $pkgdir/opt/$pkgname/bin/pycharm.png $pkgdir/usr/share/pixmaps/pycharm.png

  mkdir -p $pkgdir/usr/bin
  ln -s /opt/pycharm-community/bin/pycharm.sh $pkgdir/usr/bin/pycharm
}

# vim:set ts=2 sw=2 et:
