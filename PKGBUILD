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

prepare() {
  # Patch out directory check
  sed -i "s/build: checkinstalldir/build:/g" "${srcdir}/pakcs-${pkgver}/Makefile"
}

build() {
  echo "Building"
}

package() {
  echo "Packaging"
}
