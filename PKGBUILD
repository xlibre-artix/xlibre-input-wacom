# Maintainer: artist for Artix Linux and XLibre <artist@artixlinux.org>

pkgname=xlibre-input-wacom
pkgver=25.0.0
pkgrel=6
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
makedepends+=('meson>=0.51.0' 'gobject-introspection'
              # for tests:
              'python-libevdev' 'python-pytest' 'python-yaml' 'python-gobject' 'python-attrs')
provides+=('x11win-input-wacom')

build() {
  arch-meson ${_pkgname}-xlibre-${_pkgname}-${pkgver} build \
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

sha256sums=('12878547b271f4e59ecd5098f935d4c0bd4560d0c2a3a667385d916bc63d00af')

