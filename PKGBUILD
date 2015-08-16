# Maintainer : Alucryd <alucryd at gmail dot com>

pkgname=higan-gtk
pkgver=092
pkgrel=2
pkgdesc="Nintendo multi-system emulator - GTK version"
arch=('i686' 'x86_64')
url="http://code.google.com/p/higan/"
license=('GPL3')
depends=('xdialog' 'libpulse' 'libao' 'libgl' 'libxv' 'openal' 'sdl')
makedepends=('mesa')
optdepends=('beat: Delta patcher')
conflicts=('higan-qt')
source=("http://higan.googlecode.com/files/higan_v${pkgver}-source.tar.xz" "http://higan.googlecode.com/files/purify_v03-source.tar.xz" 'higan' 'higan.desktop' 'purify.desktop')
sha1sums=('a205005f118f6e138065af6e0d14ed990b8f1ce1'
          '2aa16f492c879d2cc1d4ffa28f4729a5ed5bb1c3'
          '931baedc3bbdd343d2decda72c13b7d0efdcba3c'
          '55f4de0a65d6428840d013f9b003d88158c131c7'
          '093643998a2fb676d795c316e35d49cf6293ce86')

# Profiles (remove as needed)
_profiles="accuracy balanced performance"

build() {
# Compile libananke
  cd "${srcdir}"/purify_v03-source/ananke
  make compiler=gcc platform=x phoenix=gtk

# Compile purify
  cd "${srcdir}"/purify_v03-source/purify
  make compiler=gcc platform=x link='-s -lX11 -ldl -Wl,-export-dynamic' phoenix=gtk

# Compile higan
  cd "${srcdir}"/higan_v${pkgver}-source/higan
  for _profile in ${_profiles} ; do
    make compiler=gcc platform=x target=ethos phoenix=gtk profile=${_profile}
    mv out/higan out/higan-${_profile}
    make clean
  done
}

package() {
# Install common files
  cd "${srcdir}"/higan_v${pkgver}-source
  install -dm 755 "${pkgdir}"/usr/{bin,lib,share/{applications,pixmaps,higan}}
  install -m 755 "${srcdir}"/higan "${pkgdir}"/usr/bin/higan
  install -m 644 "${srcdir}"/higan.desktop "${pkgdir}"/usr/share/applications/higan.desktop
  install -m 644 higan/data/higan.png "${pkgdir}"/usr/share/pixmaps/higan.png
  cp -dr --no-preserve=ownership higan/{profile/*,data/cheats.bml} "${pkgdir}"/usr/share/higan/
  cp -dr --no-preserve=ownership shaders "${pkgdir}/usr/share/higan/Video Shaders"

# Fix some permissions
  find "${pkgdir}"/usr/share/higan/ -type d -exec chmod 755 {} +
  find "${pkgdir}"/usr/share/higan/ -type f -exec chmod 644 {} +

# Install libananke
  cd "${srcdir}"/purify_v03-source/ananke
  install -m 644 libananke.so "${pkgdir}"/usr/lib/libananke.so.1
  ln -s /usr/lib/libananke.so.1 "${pkgdir}"/usr/lib/libananke.so

# Install purify
  cd "${srcdir}"/purify_v03-source/purify
  install -m 755 purify "${pkgdir}"/usr/bin/purify
  install -m 644 "${srcdir}"/purify.desktop "${pkgdir}"/usr/share/applications/purify.desktop

# Install higan
  cd "${srcdir}"/higan_v${pkgver}-source/higan
  for _profile in ${_profiles} ; do
    install -m 755 out/higan-${_profile} "${pkgdir}"/usr/bin/higan-${_profile}
  done
}

# vim: ts=2 sw=2 et:
