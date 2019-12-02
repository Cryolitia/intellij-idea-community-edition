# Maintainer: Lukas Jirkovsky <l.jirkovsky@gmail.com>
# Maintainer: Levente Polyak <anthraxx[at]archlinux[dot]org>
# Maintainer: Maxime Gauduin <alucryd@archlinux.org>

pkgname=intellij-idea-community-edition
pkgver=2019.3
_build=193.5233.102
pkgrel=1
epoch=2
pkgdesc='IDE for Java, Groovy and other programming languages with advanced refactoring features'
url='https://www.jetbrains.com/idea/'
arch=('x86_64')
license=('Apache')
backup=('usr/share/idea/bin/idea.vmoptions'
        'usr/share/idea/bin/idea64.vmoptions')
depends=('giflib' 'java-environment=11' 'python' 'sh' 'ttf-font' 'libdbusmenu-glib')
makedepends=('ant' 'kotlin' 'git' 'java8-openjfx' 'java-environment=8')
source=("git+https://github.com/JetBrains/intellij-community.git#tag=idea/${_build}"
        idea-android::"git+https://github.com/JetBrains/android#tag=idea/${_build}"
        idea-adt-tools-base::"git://git.jetbrains.org/idea/adt-tools-base.git#tag=idea/${_build}"
        idea-build.patch
        idea.desktop
        idea.sh)
sha256sums=('SKIP'
            'SKIP'
            'SKIP'
            '666e6128995b3470516dcda3815cd4b11102a20e0835b82446190c495f25cd25'
            '049c4326b6b784da0c698cf62262b591b20abb52e0dcf869f869c0c655f3ce93'
            '5d4f998bec3249165b0e62b5e51e9b7fbe383bb9f1178ffefe070f2c9872eebb')

prepare() {
  cd intellij-community
  patch -Np1 -i ../idea-build.patch
  echo ${_build} > build.txt
  sed '/def targetOs =/c def targetOs = "linux"' -i build/dependencies/setupJbre.gradle
  sed '/String targetOS/c   String targetOS = OS_LINUX' -i platform/build-scripts/groovy/org/jetbrains/intellij/build/BuildOptions.groovy
  # build system doesn't like symlinks
  mv "${srcdir}"/idea-android android
  mv "${srcdir}"/idea-adt-tools-base android/tools-base
}

build() {
  cd intellij-community
  unset _JAVA_OPTIONS
  export JAVA_HOME=/usr/lib/jvm/java-8-openjdk
  export PATH="${JAVA_HOME}/bin:${PATH}"
  ant build
  tar -xf out/idea-ce/artifacts/ideaIC-${_build}-no-jbr.tar.gz -C "${srcdir}"
}

package() {
  cd idea-IC-${_build}

  # workaround FS#40934
  sed -i 's/lcd/on/' bin/*.vmoptions

  rm -rf bin/fsnotifier-arm lib/libpty/linux/x86

  install -dm 755 "${pkgdir}"/usr/share/{licenses,pixmaps,idea,icons/hicolor/scalable/apps}
  cp -dr --no-preserve='ownership' bin lib plugins redist "${pkgdir}"/usr/share/idea/
  cp -dr --no-preserve='ownership' license "${pkgdir}"/usr/share/licenses/idea
  ln -s /usr/share/idea/bin/idea.png "${pkgdir}"/usr/share/pixmaps/
  ln -s /usr/share/idea/bin/idea.svg "${pkgdir}"/usr/share/icons/hicolor/scalable/apps/
  install -Dm 644 ../idea.desktop -t "${pkgdir}"/usr/share/applications/
  install -Dm 755 ../idea.sh "${pkgdir}"/usr/bin/idea
  install -Dm 644 build.txt -t "${pkgdir}"/usr/share/idea
}

# vim: ts=2 sw=2 et:
