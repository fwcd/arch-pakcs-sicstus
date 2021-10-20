pkgname=pakcs-sicstus
pkgver=3.3.0
pkgrel=1
pkgdesc="Portland Aachen Kiel Curry System (using SICStus Prolog)"
arch=("x86_64")
license=("custom")
depends=("sicstus")
makedepends=("stack" "rlwrap")
source=("https://www.informatik.uni-kiel.de/~pakcs/download/pakcs-${pkgver}-src.tar.gz")
sha1sums=("39ef38b074de2e21f06e7801f724efbca1bfbf55")

# Based on https://aur.archlinux.org/packages/pakcs/

prepare() {
  # Patch out directory check
  sed -i "s|build: checkinstalldir|build:|g" "${srcdir}/pakcs-${pkgver}/Makefile"
}

build() {
  cd "${srcdir}/pakcs-${pkgver}"
  make DISTPKGINSTALL=yes \
       CURRYLIBSDIR="${PWD}/lib" \
       CURRYTOOLSDIR="${PWD}/currytools" \
       PAKCSINSTALLDIR="/opt/pakcs"
}

package() {
  echo "Packaging"
  # TODO
}
