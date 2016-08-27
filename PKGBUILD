# Maintainer: Joost Bremmer <toost dot b at gmail dot com>
# Contributor: carstene1ns <url/mail: arch carsten-teibes de>

pkgname=trackma-git
pkgver=0.6.1.r5.gc3be5ff
pkgrel=1
pkgdesc="A lightweight and simple program for updating and using lists on several media tracking websites."
arch=('any')
url="http://z411.github.io/trackma/"
license=('GPL3')
depends=('python'
    'lsof')

makedepends=('python'
    'python-setuptools'
    'desktop-file-utils'
    'git'
    )
optdepends=('python-gobject: GTK frontend'
    'python-cairo:   GTK frontend'
    'python-pillow:  thumbnail images for GUI frontends'
    'python-pyqt5:   Qt frontend'
    'python-urwid:   ncurses frontend'
    'python-pyinotify: instant media recognition')

source=(${pkgname}::"git+https://github.com/z411/${pkgname%-git}.git"
    "${pkgname%-git}-curses.desktop"
    "${pkgname%-git}-gtk.desktop"
    "${pkgname%-git}-qt.desktop")

sha256sums=('SKIP'
    'dfa7486230a44625406309437741cf31ab386fbb0d94907ed15aaa0b942e248a'
    '87fdf7251a244fd8a482f46e5b2dfd5fd1460d1fb38b47fff95478fe688cbdbd'
    '54281a11092b1d2737b6bca21c692ea9a63d7a2b85969124fd25e88170866799')

#old package name.
conflicts=('wmal-git')
replaces=('wmal-git')

pkgver() {
  cd ${pkgname}
  git describe --long --tags | sed -r 's/([^-]*-g)/r\1/;s/-/./g;s/v//g'
}

prepare() {
  warning "Fixing setuptools"
  #(temporarily) remove 'gi' from setuptools requirements.
  sed --in-place=.orig "s/'gi'/'pygobject'/g" "${srcdir}/trackma-git/setup.py"
}

package() {
  cd ${pkgname}
  python setup.py install --prefix=/usr --root="$pkgdir/" --optimize=1

  install -Dvm644 "${pkgname%-git}/data/icon.png" \
  "${pkgdir}/usr/share/pixmaps/${pkgname%-git}.png"

  install -Dvm644 "${srcdir}/trackma-curses.desktop" \
  "${pkgdir}/usr/share/applications/${pkgname%-git}-curses.desktop"

  install -vm644  "${srcdir}/trackma-gtk.desktop" \
  "${pkgdir}/usr/share/applications/${pkgname%-git}-gtk.desktop"

  install -vm644  "${srcdir}/trackma-qt.desktop" \
  "${pkgdir}/usr/share/applications/${pkgname%-git}-qt.desktop"
}

# vim: sw=2 ts=2 tw=80 et:
