# Maintainer: Alexander Rødseth <rodseth@gmail.com>
# Contributor: spambanane <happy.house@gmx.de>
# Contributor: Matteo <matteo.dek@gmail.com>

pkgname=golangide
pkgver=14.0
pkgrel=1
_hgrev=828
pkgdesc='Simple IDE for Go to edit code and build projects'
arch=('x86_64' 'i686')
url='http://code.google.com/p/golangide/'
depends=('go' 'libpng12')
makedepends=('gendesk')
license=('LGPL')
options=('!strip')

_name=('Golang IDE')
_genericname=('Integrated development environment')

if [ "$CARCH" == "x86_64" ]; then
  source=("http://$pkgname.googlecode.com/files/liteidex${pkgver}.linux-amd64.hg${_hgrev}.tar.bz2"
          'golangide.png'
          'golangide.sh')
  sha256sums=('8f166ae5b97c4484bfa47932a716e28c78547944db4f2d0e222d21694c510e5b'
            '47c52b22326034bd3d6a7b11b05a53c8b3838c08e145171cf5cad2ca00260697'
            '8054157f7d1b61a9d97c045f271f935636feb4939249d1bf1ab1b39f18a15207')
else
  source=("http://$pkgname.googlecode.com/files/liteidex${pkgver}.linux-386.hg${_hgrev}.tar.bz2"
          'golangide.png'
          'golangide.sh')
  sha256sums=('3b2e565aecc141affc7d10d3aa3e2efe06866559091690b6fe13b9a664eb81c0'
            '47c52b22326034bd3d6a7b11b05a53c8b3838c08e145171cf5cad2ca00260697'
            '8054157f7d1b61a9d97c045f271f935636feb4939249d1bf1ab1b39f18a15207')
fi

build() {
  cd "$srcdir"
  gendesk -n

  cd "liteide"
  msg2 "Fixing insecure RPATH..."
  find . -name "*.so" -type f -exec sed -i 's|/home/win|/usr/lib/|g' {} \;
  find . -name liteide -type f -exec sed -i 's|/home/win|/usr/lib/|g' {} \;
}

package() {
  cd "$srcdir/liteide"

  msg2 "Creating directories..."
  mkdir -p "$pkgdir/usr/lib/liteide"
  mkdir -p "$pkgdir/usr/share/liteide"
  mkdir -p "$pkgdir/usr/share/doc/$pkgname"

  msg2 "Packaging executables..."
  for binary in goastview goapi goexec godocview liteide; do
    install -Dm755 "bin/$binary" "$pkgdir/usr/bin/$binary"
  done
  install -Dm755 "$srcdir/$pkgname.sh" "$pkgdir/usr/bin/$pkgname"

  msg2 "Packaging resources..."
  cp -r share/liteide/* "$pkgdir/usr/share/liteide"

  msg2 "Packaging libraries and plugins..."
  cp -r lib/liteide/* "$pkgdir/usr/lib/liteide"

  msg2 "Packaging license and license exception..."
  install -Dm644 LICENSE.LGPL \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE.LGPL"
  install -Dm644 LGPL_EXCEPTION.TXT \
    "$pkgdir/usr/share/licenses/$pkgname/LGPL_EXCEPTION.TXT"

  cd ..

  msg2 "Packaging menu entry and icon..."
  install -Dm644 "$pkgname.desktop" \
    "$pkgdir/usr/share/applications/$pkgname.desktop"
  install -Dm644 "$pkgname.png" \
    "$pkgdir/usr/share/pixmaps/$pkgname.png"

  msg2 "Cleaning up..."
  rm -rf "$pkgdir/usr/share/$pkgname/doc"
}

# vim:set ts=2 sw=2 et:
