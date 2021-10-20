pkgname=pakcs-sicstus
pkgver=3.3.0
pkgrel=1
pkgdesc="Portland Aachen Kiel Curry System (using SICStus Prolog)"
arch=("x86_64")
license=("custom")
depends=("sicstus")
conflicts=("pakcs")
makedepends=("stack" "rlwrap")
source=("https://www.informatik.uni-kiel.de/~pakcs/download/pakcs-${pkgver}-src.tar.gz" "skip-lc-all.patch")
sha1sums=("39ef38b074de2e21f06e7801f724efbca1bfbf55" "799603efc36da20c31c08d1624b25ab83a4185ae")

_src_path="pakcs-${pkgver}"
_install_path="opt/${pkgname}"
_install_root="/${install_path}"

# Based on https://aur.archlinux.org/packages/pakcs/

prepare() {
  _src_root="${srcdir}/${_src_path}"

  # Patch out directory check
  sed -i "s|build: checkinstalldir|build:|g" "${_src_root}/Makefile"

  # Patch out locale updates
  patch -p1 -d "${_src_root}" -i "${srcdir}/skip-lc-all.patch"
}

build() {
  _src_root="${srcdir}/${_src_path}"
  _dist_root="${pkgdir}/${_install_path}"

  cd "${_src_root}"
  make SICSTUSPROLOG="$(readlink -f $(which sicstus))"
       DISTPKGINSTALL=yes \
       CURRYLIBSDIR="${PWD}/lib" \
       CURRYTOOLSDIR="${PWD}/currytools" \
       PAKCSINSTALLDIR="${_install_root}"
}

package() {
  _src_root="${srcdir}/${_src_path}"
  _dist_root="${pkgdir}/${_install_path}"

  cd "${_src_root}"

  # Include custom license
  install -Dm 644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

  # Copy distro files
  mkdir -p "$_dist_root"
  for dir in bin lib src tools scripts currytools examples; do
    cp -r "$dir" "${_dist_root}/"
  done

  # Copy frontend
  _frontend_root="${_dist_root}/frontend"
  mkdir -p "${_frontend_root}"
  cp -r "frontend/bin" "$_frontend_root"

  # Include default config file
  install -Dm 644 pakcsrc.default "${_dist_root}/"

  # Include man pages
  install -Dm 644 man/*.1 -t "${pkgdir}/usr/share/man/man1/"

  # Patch installation paths
  sed -i "s|${_src_root}|${_install_root}|g" "${_dist_root}/bin/pakcs"
  sed -i "s|${_src_root}|${_install_root}|g" "${_dist_root}/currytools/optimize/.cpm/CURRYPATH_CACHE"

  # Link binaries to /usr/bin
  mkdir -p "${pkgdir}/usr/bin"
  ln -s "${_install_root}/bin/cypm" "${pkgdir}/usr/bin/pakcs-cypm"
  ln -s "${_install_root}/bin/pakcs" "${pkgdir}/usr/bin/pakcs"
}
