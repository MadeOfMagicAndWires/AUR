# Maintainer : Daniel Bermond < yahoo-com: danielbermond >
# Contributor: Sidney Crestani <sidneycrestani@archlinux.net>
# Contributor: sxe <sxxe@gmx.de>

pkgname=wine-git
pkgver=2.1.r285.gd00f7315e0
pkgrel=2
pkgdesc='A compatibility layer for running Windows programs (git version)'
arch=('i686' 'x86_64')
url='https://www.winehq.org'
license=('LGPL')
_depends=(
    'fontconfig'            'lib32-fontconfig'
    'lcms2'                 'lib32-lcms2'
    'libxml2'               'lib32-libxml2'
    'libxcursor'            'lib32-libxcursor'
    'libxrandr'             'lib32-libxrandr'
    'libxdamage'            'lib32-libxdamage'
    'libxi'                 'lib32-libxi'
    'gettext'               'lib32-gettext'
    'freetype2'             'lib32-freetype2'
    'glu'                   'lib32-glu'
    'libsm'                 'lib32-libsm'
    'gcc-libs'              'lib32-gcc-libs'
    'libpcap'               'lib32-libpcap'
    'desktop-file-utils'
)
makedepends=('git' 'autoconf' 'ncurses' 'bison' 'perl' 'fontforge' 'flex'
    'gcc>=4.5.0-2'          'gcc-multilib>=4.5.0-2'
    'giflib'                'lib32-giflib'
    'libpng'                'lib32-libpng'
    'gnutls'                'lib32-gnutls'
    'libxinerama'           'lib32-libxinerama'
    'libxcomposite'         'lib32-libxcomposite'
    'libxmu'                'lib32-libxmu'
    'libxxf86vm'            'lib32-libxxf86vm'
    'libldap'               'lib32-libldap'
    'mpg123'                'lib32-mpg123'
    'openal'                'lib32-openal'
    'v4l-utils'             'lib32-v4l-utils'
    'libpulse'              'lib32-libpulse'
    'alsa-lib'              'lib32-alsa-lib'
    'libxcomposite'         'lib32-libxcomposite'
    'mesa'                  'lib32-mesa'
    'libgl'                 'lib32-libgl'
    'opencl-icd-loader'     'lib32-opencl-icd-loader'
    'libxslt'               'lib32-libxslt'
    'gst-plugins-base-libs' 'lib32-gst-plugins-base-libs'
    'samba'
    'opencl-headers'
)
optdepends=(
    'giflib'                'lib32-giflib'
    'libpng'                'lib32-libpng'
    'libldap'               'lib32-libldap'
    'gnutls'                'lib32-gnutls'
    'mpg123'                'lib32-mpg123'
    'openal'                'lib32-openal'
    'v4l-utils'             'lib32-v4l-utils'
    'libpulse'              'lib32-libpulse'
    'alsa-plugins'          'lib32-alsa-plugins'
    'alsa-lib'              'lib32-alsa-lib'
    'libjpeg-turbo'         'lib32-libjpeg-turbo'
    'libxcomposite'         'lib32-libxcomposite'
    'libxinerama'           'lib32-libxinerama'
    'ncurses'               'lib32-ncurses'
    'opencl-icd-loader'     'lib32-opencl-icd-loader'
    'libxslt'               'lib32-libxslt'
    'gst-plugins-base-libs' 'lib32-gst-plugins-base-libs'
    'cups'
    'samba'
    'dosbox'
)
options=('staticlibs')
source=("$pkgname"::'git://source.winehq.org/git/wine.git'
        '30-win32-aliases.conf'
        '0001-winhlp32-Workaround-a-bug-in-Flex.patch')
sha256sums=('SKIP'
            '9901a5ee619f24662b241672a7358364617227937d5f6d3126f70528ee5111e7'
            '140f489aeea9ff9bd605eb5b6f763fabeea51897ffa1138a6e5b01c4784b646c')

if [ "$CARCH" = 'i686' ] 
then
    # strip lib32 etc. on i686
    _depends=(${_depends[@]/*32-*/})
    makedepends=(${makedepends[@]/*32-*/} ${_depends[@]})
    makedepends=(${makedepends[@]/*-multilib*/})
    optdepends=(${optdepends[@]/*32-*/})
else
    makedepends=(${makedepends[@]} ${_depends[@]})
    provides=("bin32-wine=$pkgver" "wine=$pkgver" "wine-wow64=$pkgver")
    conflicts=('bin32-wine' 'wine' 'wine-wow64')
    replaces=('bin32-wine')
fi

prepare() {
    # https://bugs.winehq.org/show_bug.cgi?id=42132
    cd "$pkgname"
    patch -p1 -i "${srcdir}/0001-winhlp32-Workaround-a-bug-in-Flex.patch"
    
    # fix path of opencl headers
    sed 's|OpenCL/opencl.h|CL/opencl.h|g' -i "$pkgname"/configure*
}

pkgver() {
    cd "$pkgname"
    git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g;s/^wine.//;s/^v//'
}

build() {
    # delete old build dirs (from previous builds) and make new ones
    rm    -rf "$pkgname"-{32,64}-build
    mkdir -p  "$pkgname"-32-build
    
    # workaround for FS#55128
    # https://bugs.archlinux.org/task/55128
    # https://bugs.winehq.org/show_bug.cgi?id=43530
    export CFLAGS="${CFLAGS/-fno-plt/}"
    export LDFLAGS="${LDFLAGS/,-z,now/}"
    
    # build wine 64-bit
    # (according to the wine wiki, this 64-bit/32-bit building order is mandatory)
    if [ "$CARCH" = 'x86_64' ] 
    then
        msg2 'Building Wine-64...'

        mkdir "$pkgname"-64-build
        cd    "$pkgname"-64-build
        
        ../"$pkgname"/configure \
                          --prefix='/usr' \
                          --libdir='/usr/lib' \
                          --with-x \
                          --with-gstreamer \
                          --enable-win64
        make
        
        local _wine32opts=(
                    '--libdir=/usr/lib32'
                    "--with-wine64=${srcdir}/${pkgname}-64-build"
        )
        
        export PKG_CONFIG_PATH='/usr/lib32/pkgconfig'
    fi
    
    # build wine 32-bit
    msg2 'Building Wine-32...'
    
    cd "${srcdir}/${pkgname}"-32-build
    
    ../"$pkgname"/configure \
                      --prefix='/usr' \
                      --with-x \
                      --with-gstreamer \
                      ${_wine32opts[@]}
    make
}

package() {
    depends=(${_depends[@]})
    
    # package wine 32-bit
    # (according to the wine wiki, this reverse 32-bit/64-bit packaging order is important)
    msg2 'Packaging Wine-32...'
    
    cd "$pkgname"-32-build
    
    if [ "$CARCH" = 'i686' ] 
    then
        make prefix="$pkgdir/usr" install
    else
        make prefix="$pkgdir/usr" \
             libdir="$pkgdir/usr/lib32" \
             dlldir="$pkgdir/usr/lib32/wine" install
    
        # package wine 64-bit
        msg2 'Packaging Wine-64...'
        
        cd "${srcdir}/${pkgname}"-64-build
        
        make prefix="$pkgdir/usr" \
             libdir="$pkgdir/usr/lib" \
             dlldir="$pkgdir/usr/lib/wine" install
    fi
    
    # font aliasing settings for Win32 applications
    install -d "$pkgdir"/etc/fonts/conf.{avail,d}
    install -m644 "${srcdir}/30-win32-aliases.conf" "${pkgdir}/etc/fonts/conf.avail"
    ln -s ../conf.avail/30-win32-aliases.conf "${pkgdir}/etc/fonts/conf.d/30-win32-aliases.conf"
}
