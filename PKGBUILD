# Maintainer: Lukas Jirkovsky <l.jirkovsky@gmail.com>
# Maintainer: Levente Polyak <anthraxx[at]archlinux[dot]org>
# Maintainer: Maxime Gauduin <alucryd@archlinux.org>

pkgname=intellij-idea-community-edition
pkgver=2018.2.1
_build=182.3911.36
pkgrel=2
epoch=2
pkgdesc='IDE for Java, Groovy and other programming languages with advanced refactoring features'
arch=('x86_64')
url='https://www.jetbrains.com/idea/'
license=('Apache')
backup=('usr/share/idea/bin/idea.vmoptions'
        'usr/share/idea/bin/idea64.vmoptions')
depends=('giflib' 'java-environment=8' 'python' 'sh' 'ttf-font')
makedepends=('apache-ant' 'git' 'java-openjfx')
install='idea.install'
source=("idea-${_build}.tar.gz::https://github.com/JetBrains/intellij-community/archive/idea/${_build}.tar.gz"
        "idea-android::git://git.jetbrains.org/idea/android.git#tag=idea/${_build}"
        "idea-adt-tools-base::git://git.jetbrains.org/idea/adt-tools-base.git#tag=idea/${_build}"
        'idea-build.patch'
        'idea.desktop'
        'idea.sh')
sha256sums=('4556dbf0099fde8115b05bdc76e80c82263d9c3ef98c9b112c48d610f2fac509'
            'SKIP'
            'SKIP'
            '3793e8125abb05b1580919017469ada2563a2e5972a8d74666557df60d270cfd'
            'fa9e3cba5e26a7e01cecda867f23467322db123c5553dfbb4f14aae034ccbed7'
            '9e2155dd4d352b2410fc689236b15d5c3cb9937d82b50d39ec8b8dbcdfa40de1')

prepare() {
  cd intellij-community-idea-${_build}

  patch -Np1 -i ../idea-build.patch
  echo ${_build} > build.txt
  ln -s "${srcdir}"/idea-android android
  ln -s "${srcdir}"/idea-adt-tools-base android/tools-base
}

build() {
  cd intellij-community-idea-${_build}

  unset _JAVA_OPTIONS

  ant build
  tar -xf out/idea-ce/artifacts/ideaIC-${_build}-no-jdk.tar.gz -C "${srcdir}"
}

package() {
  cd idea-IC-${_build}

  # workaround FS#40934
  sed -i 's/lcd/on/' bin/*.vmoptions

  rm -rf bin/fsnotifier{,-arm} lib/libpty/linux/x86

  install -dm 755 "${pkgdir}"/usr/share/{licenses,pixmaps,idea}
  cp -dr --no-preserve='ownership' bin lib plugins redist "${pkgdir}"/usr/share/idea/
  cp -dr --no-preserve='ownership' license "${pkgdir}"/usr/share/licenses/idea
  ln -s /usr/share/idea/bin/idea.png "${pkgdir}"/usr/share/pixmaps/
  install -Dm 644 ../idea.desktop -t "${pkgdir}"/usr/share/applications/
  install -Dm 755 ../idea.sh "${pkgdir}"/usr/bin/idea
}

# vim: ts=2 sw=2 et:

