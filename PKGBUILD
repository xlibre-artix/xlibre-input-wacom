# Maintainer: artist for Artix Linux and XLibre <artist@artixlinux.org>

pkgname=xlibre-input-wacom
pkgver=25.0.0
pkgrel=7
pkgdesc="XLibre fork of X.Org Wacom tablet driver"
arch=(x86_64 aarch64)
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
  case "$CARCH" in
    "x86_64")
      CFLAGS=" -march=x86-64"
      ;;
    "aarch64")
      CFLAGS=" -march=armv8-a"
      ;;
    *)
      CFLAGS=" -march=native"
      ;;
  esac
  CFLAGS+=" -mtune=generic -O2 -pipe -fexceptions -Wp,-D_FORTIFY_SOURCE=3 -Wformat -Werror=format-security"
  CFLAGS+=" -fstack-clash-protection -fno-omit-frame-pointer -mno-omit-leaf-frame-pointer"
  LDFLAGS=" -Wl,-O1 -Wl,--sort-common -Wl,--as-needed -Wl,-z,lazy -Wl,-z,relro -Wl,-z,pack-relative-relocs"
  if [[ $CARCH != 'aarch64' ]]; then
    CFLAGS+=" -fcf-protection"
  fi
  if [[ "$_pkgname" == *"xf86-input"* ]]; then
    CFLAGS+=" -fno-plt"
    LDFLAGS+=" -Wl,-z,now"
  fi
  if [[ "$_pkgname" == *"xf86-video-intel"* ]]; then
    CFLAGS+=" -fno-lto"
    LDFLAGS+=" -fno-lto"
  fi
  CXXFLAGS="${CFLAGS} -Wp,-D_GLIBCXX_ASSERTIONS"
  export CFLAGS="${CFLAGS}"
  export CXXFLAGS="${CXXFLAGS}"
  export LDFLAGS="${LDFLAGS}"

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

