# Maintainer: Michal Bok <michal.bok@airslate.com>
pkgname=konqix
pkgver=0.1.4
pkgrel=1
pkgdesc="Qt 6 instant messenger built on libpurple"
arch=('x86_64')
url="https://github.com/radek-bucek/konqix"
license=('GPL-2.0-or-later')
depends=('qt6-base' 'libpurple' 'kwindowsystem' 'hunspell')
optdepends=('hunspell-en_us: English dictionary for the spell-check input overlay')
makedepends=('cmake' 'pkgconf' 'desktop-file-utils')
source=("$pkgname-$pkgver.tar.gz::$url/archive/refs/tags/v$pkgver.tar.gz")
sha256sums=('220b21e29260dfe883397f685c810f865ea4590fa4dd6184aae38b6ab673bd2f')

build() {
    cmake -B build -S "$pkgname-$pkgver" \
        -DCMAKE_BUILD_TYPE=None \
        -DCMAKE_INSTALL_PREFIX=/usr
    cmake --build build
}

check() {
    desktop-file-validate "$pkgname-$pkgver/packaging/com.konqix.Konqix.desktop"
}

package() {
    DESTDIR="$pkgdir" cmake --install build
}
