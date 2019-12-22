# Maintainer: Tarmo Heiskanen <turskii@gmail.com>
# Contributor: mar77i <mar77i at mar77i dot ch>
# Contributor: Gaetan Bisson <bisson@archlinux.org>
# Contributor: Scytrin dai Kinthra <scytrin@gmail.com>

pkgname=st-git
pkgver=0.8.2.r20.g8386642
pkgrel=1
pkgdesc='Simple virtual terminal emulator for X'
url='https://st.suckless.org/'
arch=('i686' 'x86_64')
license=('MIT')
depends=('libxft')
makedepends=('ncurses' 'libxext' 'git')
source=('git://git.suckless.org/st'
        'https://st.suckless.org/patches/scrollback/st-scrollback-0.8.diff'
        'https://st.suckless.org/patches/scrollback/st-scrollback-mouse-0.8.diff'
        'https://st.suckless.org/patches/scrollback/st-scrollback-mouse-altscreen-20190131-e23acb9.diff'
        'st-font-and-colors.diff')
sha1sums=('SKIP'
          '623f474b62fb08e0b470673bf8ea947747e1af8b'
          '46e92d9d3f6fd1e4f08ed99bda16b232a1687407'
          '0743f3736ff18be535e25c0916a89e5eed9d5f4f'
          'ee4d805c199d7b017cf36b8ccc51bd076aacc6ee')
provides=("st")
conflicts=("st")


pkgver() {
    cd "${srcdir}/st"
    git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
    # "Fix" st-scrollback patch
    # The diff assumes a line in config.def.h which is not present
    sed -i st-scrollback-0.8.diff -e '8d'
    sed -i st-scrollback-0.8.diff -e 's/@@ -178,6 +178,8 @@/@@ -178,5 +178,7 @@/'

    cd "${srcdir}/st"

    echo 'Copying config.def.h to $startdir...'
    cp config.def.h "${startdir}/"

    echo 'Copying config.h from $startdir if it exists...'
    [ -f "${startdir}/config.h" ] && cp "${startdir}/config.h" . || true

	sed -e '/char worddelimiters/s/= .*/= " '"'"'`\\\"()[]{}<>|";/' -i config.def.h
}

build() {
    cd "${srcdir}/st"

    make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
    cd "${srcdir}/st"

    make PREFIX=/usr DESTDIR="${pkgdir}" TERMINFO="/dev/null" install
    install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
    install -Dm644 README "${pkgdir}/usr/share/doc/${pkgname}/README"
}
