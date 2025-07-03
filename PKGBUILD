# Maintainer: Leonid Pilyugin <l.pilyugin04@gmail.com>>

pkgname=kawaii-calamares
pkgver=3.3.14
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
    "$pkgname-$pkgver.tar.gz::https://github.com/LeonidPilyugin/$pkgname/releases/download/v$pkgver/kawaii.patch"
    "calamares-$pkgver.tar.gz::$url/archive/v$pkgver.tar.gz"
)
sha256sums=(
    '8ae446d41f6000bf60443db90feca826b0262d5ded0f58be6e07f29ba5fd2027'
    '523a0e4cfefbe471d9a6a5d874018fe282a4ce3667ff560bd97190339be6b592'
)

prepare() {
  cd "$_pkgsrc"
  git apply ../kawaii.patch
}

build() {
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
    -S "$_pkgsrc"
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
