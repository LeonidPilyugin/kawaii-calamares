# Maintainer: Leonid Pilyugin <l.pilyugin04@gmail.com>>

pkgname=kawaii-calamares
pkgver=3.3.1
pkgrel=1
pkgdesc='Kawaii installer for MenheraOS'
url='https://github.com/LeonidPilyugin/kawaii-calamares'
arch=('x86_64')
depends=(
	'ckbcomp'
	'efibootmgr'
	'gtk-update-icon-cache'
	'hwinfo'
	'icu'
	'kpmcore>=24.01.75'
	'libpwquality'
	'mkinitcpio-openswap'
	'squashfs-tools'
	'yaml-cpp'
	)
license=(
	'BSD-2-Clause'
	'CC-BY-4.0'
	'CC0-1.0'
	'GPL-3.0-or-later'
	'LGPL-2.1-only'
	'LGPL-3.0-or-later'
	'MIT'
	)
source=("$pkgname-$pkgver.tar.gz::https://github.com/LeonidPilyugin/$pkgname/releases/download/v$pkgver/files.tar.gz")
sha256sums=('a81e86953aa9bb3fcecc5077cd41c21d6d7ac84745ce61384853e2b6734ea4eb')

prepare() {
    cd ${srcdir}/files
    sed -i -e 's/"Install configuration files" OFF/"Install configuration files" ON/' CMakeLists.txt
    sed -i -e 's/etc\/sddm.conf/etc\/sddm.conf.d\/kde_settings.conf/' src/modules/displaymanager/main.py
    # patches here
    # change version
    _ver="$(cat CMakeLists.txt | grep -m3 -e "  VERSION" | grep -o "[[:digit:]]*" | xargs | sed s'/ /./g')"
    printf 'Version: %s-%s' "${_ver}" "${pkgrel}"
    sed -i -e "s|\${CALAMARES_VERSION_MAJOR}.\${CALAMARES_VERSION_MINOR}.\${CALAMARES_VERSION_PATCH}|${_ver}-${pkgrel}|g" CMakeLists.txt
    sed -i -e "s|CALAMARES_VERSION_RC 1|CALAMARES_VERSION_RC 0|g" CMakeLists.txt

}

build() {
    cd ${srcdir}/files
    mkdir -p build
    cd build
        cmake .. \
              -DCMAKE_BUILD_TYPE=Release \
              -DCMAKE_INSTALL_PREFIX=/usr \
              -DCMAKE_INSTALL_LIBDIR=lib \
              -DWITH_PYTHONQT:BOOL=ON \
              -DBoost_NO_BOOST_CMAKE=ON \
              -DSKIP_MODULES="tracking webview interactiveterminal initramfs \
                              initramfscfg dracut dracutlukscfg \
                              dummyprocess dummypython dummycpp \
                              dummypythonqt services-openrc"
        make
}

package() {
    cd ${srcdir}/files/build
    make DESTDIR="$pkgdir" install
}
