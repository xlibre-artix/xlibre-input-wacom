# Maintainer: artist for Artix Linux

pkgname=xlibre-input-wacom
pkgver=1.2.3.3
pkgrel=5
pkgdesc="XLibre fork of X.Org Wacom tablet driver"
arch=(x86_64)
license=('GPL-2.0-or-later')
_pkgname="${pkgname//xlibre/xf86}"
url="https://github.com/X11Libre/${_pkgname}"
depends=("xlibre-xserver>=${pkgver%.*}" 'glibc')
makedepends=("xlibre-xserver-devel>=${pkgver%.*}" 'xorgproto')
conflicts=("${_pkgname}")
provides=("${_pkgname}")
source=("${url}/archive/refs/tags/xlibre-${_pkgname}-${pkgver}.tar.gz")
groups=('xlibre-drivers')
depends+=('libxi' 'libxinerama' 'libxrandr' 'libx11')
makedepends+=('meson' 'gobject-introspection'
              # for tests:
              'python-libevdev' 'python-pytest' 'python-yaml' 'python-gobject' 'python-attrs')
provides+=('x11win-input-wacom')

build() {
  artix-meson ${_pkgname}-xlibre-${_pkgname}-${pkgver} build \
    -D xorg-conf-dir=/usr/share/X11/xorg.conf.d/ \
    -D unittests=enabled

  meson configure build
  ninja -C build
}

check() {
  meson test -C build
}

package() {
  DESTDIR="$pkgdir" ninja -C build install
  rm -r ${pkgdir}/usr/lib/systemd
}

sha256sums=('21f25957a0049cd4f001c75996137ed794714e042cb7382d1ac77e23715c51fa')

