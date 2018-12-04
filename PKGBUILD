# Maintainer: Jonathan Knapp <jaknapp8+aur@gmail.com>
# Contributor: Roasbeef
# URL: https://github.com/lightningnetwork/lnd
# Upstream: https://github.com/lightningnetwork/lnd

pkgname=('lnd-git')
pkgver=0.5.1.beta.96.g640fe255
pkgrel=1
pkgdesc='The Lighting Network Daemon, for secure off-chain bitcoin transactions.'
arch=('i686' 'x86_64')
url='https://github.com/lightningnetwork/lnd'
license=('MIT')
depends=('glibc')
makedepends=('git' 'go')
provides=('lnd' 'lncli')
conflicts=('lnd')
source=("$pkgname::git+https://github.com/lightningnetwork/lnd.git")
md5sums=('SKIP')

# create a fake go path directory and pushd into it
# $1 real directory
# $2 gopath directory
_fake_gopath_pushd() {
  mkdir -p "$GOPATH/src/${2%/*}"
  rm -f "$GOPATH/src/$2"
  ln -rsT "$1" "$GOPATH/src/$2"
  pushd "$GOPATH/src/$2" >/dev/null || exit
}

_fake_gopath_popd() {
  popd >/dev/null || exit
}

pkgver() {
  cd "$pkgname" || exit
  git describe --tags | sed 's/^v//;s/-/./g'
}

prepare() {
  # Create GOPATH
  mkdir -p "$srcdir/GOPATH"
}

build() {
  export GOPATH="$srcdir/GOPATH"
  _fake_gopath_pushd "$pkgname" github.com/lightningnetwork/lnd
  make && make install
  _fake_gopath_popd
}

package() {
  for _bin in lnd lncli; do
    install -Dm 755 "$srcdir/GOPATH/bin/$_bin" -t "$pkgdir/usr/bin";
  done

  install -D -m644 "$pkgname/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
