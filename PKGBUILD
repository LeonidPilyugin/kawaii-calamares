# Maintainer: Leonid Pilyugin <l.pilyugin04@gmail.com>>

pkgname=kawaii-calamares
pkgver=3.4.2
pkgrel=1
pkgdesc='Kawaii installer for MenheraOS'
url='https://codeberg.org/Calamares/calamares/releases/download/'
arch=('x86_64')
depends=(
	'ckbcomp'
	'efibootmgr'
	'gtk-update-icon-cache'
	'hwinfo'
	'icu'
	'kpmcore'
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
makedepends=('extra-cmake-modules' 'git' 'qt6-tools' 'qt6-translations')
source=(
    "kawaii.patch::https://github.com/LeonidPilyugin/$pkgname/releases/download/v$pkgver/kawaii.patch"
    "calamares-$pkgver.tar.gz::$url/v$pkgver/calamares-$pkgver.tar.gz"
)
sha256sums=(
    '7c0cb3c3c271cbaae83c8eee21f102faed0e8b10ef12e2f570bced6719ade788'
    '733bbbb00dc9f84874bd5c22960952f317ea2537565431179fa2152b2fbfdccc'
)

prepare() {
  local _pkgsrc_dir="$srcdir/calamares-$pkgver"
  cd "$_pkgsrc_dir"
  git apply ../kawaii.patch
}

build() {
  local _pkgsrc_dir="$srcdir/calamares-$pkgver"
  local _skip_modules=(
    dracut
    dracutlukscfg
    dummycpp
    dummyprocess
    dummypython
    dummypythonqt
    initramfs
    initramfscfg
    interactiveterminal
    packagechooser
    packagechooserq
    services-openrc
  )

  local _cmake_options=(
    -B build
    -S "$_pkgsrc_dir"
    -G Ninja
    -DCMAKE_BUILD_TYPE=Release
    -DCMAKE_INSTALL_PREFIX='/usr'
    -DCMAKE_INSTALL_LIBDIR='lib'
    -DWITH_QT6=ON
    -DINSTALL_CONFIG=ON
    -DSKIP_MODULES="${_skip_modules[*]}"
    -DBUILD_TESTING=OFF
    -Wno-dev
  )

  cmake "${_cmake_options[@]}"
  cmake --build build
}

package() {
  DESTDIR="$pkgdir" cmake --install build
}
