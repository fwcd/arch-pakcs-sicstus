pkgname=pakcs-sicstus
pkgver=3.3.0
pkgrel=4
pkgdesc="Portland Aachen Kiel Curry System (using SICStus Prolog)"
arch=("x86_64")
license=("custom")
depends=("sicstus" "rlwrap" "sqlite")
provides=("pakcs")
conflicts=("pakcs")
makedepends=("stack")
source=("https://www.informatik.uni-kiel.de/~pakcs/download/pakcs-$pkgver-src.tar.gz")
sha1sums=("39ef38b074de2e21f06e7801f724efbca1bfbf55")

_srcpath="pakcs-$pkgver"
_installpath="opt/$pkgname"
_installroot="/$_installpath"

# Based on https://aur.archlinux.org/packages/pakcs/

prepare() {
  # Patch out directory check
  sed -i "s|build: checkinstalldir|build:|g" "$srcdir/$_srcpath/Makefile"
}

build() {
  cd "$srcdir/$_srcpath"

  # Use custom locale since C.UTF-8 is not available on Arch
  export LC_ALL=en_US.UTF-8

  # Build PAKCS
  make SICSTUSPROLOG="$(readlink -f $(which sicstus))" \
       DISTPKGINSTALL=yes \
       CURRYLIBSDIR="$PWD/lib" \
       CURRYTOOLSDIR="$PWD/currytools" \
       PAKCSINSTALLDIR="$_installroot"
}

package() {
  _srcroot="$srcdir/$_srcpath"
  _distroot="$pkgdir/$_installpath"

  cd "$_srcroot"

  # Include custom license
  install -Dm 644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

  # Copy distro files
  mkdir -p "$_distroot"
  for dir in bin lib src tools scripts currytools examples; do
    cp -r "$dir" "$_distroot/"
  done

  # Copy frontend
  _frontendroot="$_distroot/frontend"
  mkdir -p "$_frontendroot"
  cp -r "frontend/bin" "$_frontendroot"

  # Include default config file
  install -Dm 644 pakcsrc.default "$_distroot/"

  # Include man pages
  install -Dm 644 man/*.1 -t "$pkgdir/usr/share/man/man1/"

  # Patch installation paths
  sed -i "s|$_srcroot|$_installroot|g" "$_distroot/bin/pakcs"

  # Link binaries to /usr/bin
  mkdir -p "$pkgdir/usr/bin"
  ln -s "$_installroot/bin/cypm" "$pkgdir/usr/bin/pakcs-cypm"
  ln -s "$_installroot/bin/pakcs" "$pkgdir/usr/bin/pakcs"
}
