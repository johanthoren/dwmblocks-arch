# -*- mode: shell-script;-*-
# Maintainer: Johan Thorén <johan@thoren.xyz>
pkgname=dwmblocks-jt
_pkgname=dwmblocks
builddir="$(pwd)"
pkgver=0.r61.d294ac2
pkgrel=1
pkgdesc="Johan Thorén's personal patched version of dwmblocks."
url="https://github.com/johanthoren/dwmblocks"
arch=('x86_64')
license=('ISC')
options=(zipman)
depends=('libx11' 'dwm' 'inetutils' 'ttf-symbola')
makedepends=('git')
provides=('dwmblocks')
conflicts=('dwmblocks')
source=(personal_preferences.diff
        "$_pkgname::git+https://github.com/johanthoren/dwmblocks.git")
md5sums=('SKIP'
         'SKIP')

pkgver(){
	  cd $_pkgname
	  _pkgver=0
 	  echo "${_pkgver}.r$(git rev-list --count HEAD).$(git rev-parse --short HEAD)"
}

_patch_it() {
    echo "Adding patch $1"
    patch --forward --strip=1 --input="${srcdir}/${1}"
    echo ""
}

prepare() {
    cd $_pkgname

    patches=(personal_preferences.diff)

    for p in "${patches[@]}"; do
        _patch_it "$p"
    done

  # If a blocks.h exists in the root directory, then it will be used
  # instead.
  if [ -e "${builddir}/blocks.h" ]
  then
    cp "${builddir}/blocks.h" "${srcdir}/${_pkgname}/blocks.h"
  fi
}

build() {
  cd $_pkgname
  make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
  cd $_pkgname
  make PREFIX=/usr DESTDIR="$pkgdir" install
  install -m644 -D LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

# vim:set ts=2 sw=2 et:
