<<<<<<< HEAD
# Maintainer: carstene1ns <arch carsten-teibes de> - http://git.io/ctPKG
# Contributor: Eschwartz <eschwartz93@gmail.com>
# Contributors: Ner0, Sevenseven

pkgname=qbittorrent-git
<<<<<<< HEAD
<<<<<<< HEAD
<<<<<<< HEAD
pkgver=3.2.5.r0.gc0ccf28
pkgrel=1
=======
pkgver=3.2.4.r2.g522ff3b
pkgrel=2
>>>>>>> deb755c... upgpkg: qbittorrent-git 3.2.4.r2.g522ff3b-2
=======
pkgver=3.2.5.r0.gc0ccf28
pkgrel=1
>>>>>>> 01aff63... upgpkg: qbittorrent-git 3.2.5.r0.gc0ccf28-1
pkgdesc="A bittorrent client written in C++ / Qt4 using the good libtorrent library (development version)"
=======
pkgver=3.3.1.r0.gd753988
pkgrel=1
pkgdesc="A bittorrent client powered by C++, Qt5 and the good libtorrent library (development version)"
>>>>>>> 3a7ae88... [upd] Use qt5
arch=('i686' 'x86_64')
url="http://www.qbittorrent.org/"
license=('custom' 'GPL')
depends=('libtorrent-rasterbar' 'qt5-base' 'desktop-file-utils' 'hicolor-icon-theme' 'xdg-utils')
makedepends=('boost' 'git' 'qt5-tools')
optdepends=('python: needed for torrent search tab')
conflicts=('qbittorrent' 'qbittorrent-qt4')
provides=('qbittorrent' 'qbittorrent-qt5')
install=qbittorrent.install
source=(${pkgname%-*}::"git+https://github.com/qbittorrent/qBittorrent.git#branch=v3_3_x")
md5sums=('SKIP')

pkgver() {
  cd ${pkgname%-*}

  git describe --long --tags | sed 's/^release-//;s/-/.r/;s/-/./'
}

build() {
  cd ${pkgname%-*}

  ./configure --prefix=/usr
  make
}

package() {
  cd ${pkgname%-*}

  make INSTALL_ROOT="$pkgdir/" install
  install -Dm644 COPYING "$pkgdir"/usr/share/licenses/$pkgname/COPYING
=======
# Maintainer: Hyacinthe Cartiaux <hyacinthe.cartiaux@free.fr>
# Maintainer: Eli Schwartz <eschwartz93@gmail.com>

_pkgname=https-everywhere
pkgname=firefox-extension-${_pkgname}
pkgver=5.1.4
pkgrel=1
_file=403754
pkgdesc="Plugin for firefox which ensures you are using https whenever it's possible."
license=('GPL2')
arch=('any')
url="https://www.eff.org/https-everywhere"
depends=("firefox")
makedepends=("unzip")
source=("${_pkgname}-${pkgver}.xpi::https://addons.mozilla.org/firefox/downloads/file/${_file}/${_pkgname/-/_}-${pkgver}-an+tb+sm+fx.xpi")
noextract=("${_pkgname}-${pkgver}.xpi")
sha256sums=('0e2cd19dfa5fedfbfa374971d804951ce4177a6ce44a048c6b43e19bbecf4534')

prepare() {
  cd "$srcdir"

  unzip -qqo "${_pkgname}-${pkgver}.xpi" -d "${_pkgname}-${pkgver}"
}

package() {
<<<<<<< HEAD
<<<<<<< HEAD
  cd "$srcdir"

  emid=$(sed -n '/.*<em:id>\(.*\)<\/em:id>.*/{s//\1/p;q}' install.rdf)
  local dstdir="${pkgdir}/usr/lib/firefox/browser/extensions/${emid}"

<<<<<<< HEAD
  install -d $dstdir
  cp -dpr --no-preserve=ownership * $dstdir
  rm $dstdir/https-everywhere-${_plugin_version}.xpi
  chmod -R 755 $dstdir
>>>>>>> 2b17094... Initial import
=======
  install -d "$dstdir"
  cp -dpr --no-preserve=ownership * "$dstdir"
  rm "${dstdir}/https-everywhere-${_plugin_version}-eff.xpi"

  find $pkgdir -type d -exec chmod 0755 {} \;
  find $pkgdir -type f -exec chmod 0644 {} \;
>>>>>>> a127f6a... Update to 5.1.0
=======
  cd $srcdir
  local _emid=$(sed -n '/.*<em:id>\(.*\)<\/em:id>.*/{s//\1/p;q}' install.rdf) || return 1
  test ! -z "${_emid}"
  local _file=(*.xpi)
  test "${#_file[@]}" -eq 1
  install -Dpm644 "${_file}" "${pkgdir}/usr/lib/firefox/browser/extensions/${_emid}.xpi"
>>>>>>> 0bd6f12... Pkgrel bump: new install method for the signing mechanism...
=======
  cd "${srcdir}"

  _extension_id="$(sed -n '/.*<em:id>\(.*\)<\/em:id>.*/{s//\1/p;q}' ${_pkgname}-${pkgver}/install.rdf)"
  _extension_dest="${pkgdir}/usr/lib/firefox/browser/extensions/${_extension_id}"
  # Should this extension be unpacked or not?
  if grep '<em:unpack>true</em:unpack>' ${_pkgname}-${pkgver}/install.rdf > /dev/null; then
    install -dm755 "${_extension_dest}"
    cp -R ${_pkgname}-${pkgver}/* "${_extension_dest}"
    chmod -R ugo+rX "${_extension_dest}"
  else
    install -Dm644 ${_pkgname}-${pkgver}.xpi "${_extension_dest}.xpi"
  fi
>>>>>>> dc6d74f... upgpkg: firefox-extension-https-everywhere 5.1.1-4
}
