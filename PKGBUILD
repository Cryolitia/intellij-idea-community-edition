# Maintainer: Lukas Jirkovsky <l.jirkovsky@gmail.com>
# Maintainer: Levente Polyak <anthraxx[at]archlinux[dot]org>
# Maintainer: Maxime Gauduin <alucryd@archlinux.org>
# Maintainer: Orhun Parmaksız <orhun@archlinux.org>

pkgname=intellij-idea-community-edition
pkgver=2022.2.2
_build=222.4167.29
_jrever=17
_jdkver=17
pkgrel=2
epoch=4
pkgdesc='IDE for Java, Groovy and other programming languages with advanced refactoring features'
url='https://www.jetbrains.com/idea/'
arch=('x86_64')
license=('Apache')
backup=('usr/share/idea/bin/idea64.vmoptions')
depends=('giflib' "jre${_jrever}-openjdk" 'python' 'sh' 'ttf-font' 'libdbusmenu-glib' 'fontconfig' 'hicolor-icon-theme')
makedepends=('ant' 'git' "jdk${_jdkver}-openjdk")
optdepends=(
  'lldb: lldb frontend integration'
)
source=("git+https://github.com/JetBrains/intellij-community.git#tag=idea/${_build}"
        idea-android::"git://git.jetbrains.org/idea/android.git#tag=idea/${_build}"
        idea-adt-tools-base::"git://git.jetbrains.org/idea/adt-tools-base.git#commit=17e9c8b666cac0b974b1efc5e1e4c33404f72904"
        idea.desktop
        idea.sh)
sha256sums=('SKIP'
            'SKIP'
            'SKIP'
            '049c4326b6b784da0c698cf62262b591b20abb52e0dcf869f869c0c655f3ce93'
            'd7e4a325fccd48b8c8b0a6234df337b58364e648bb9b849e85ca38a059468e71')

prepare() {
  cd intellij-community

  # build system doesn't like symlinks
  mv "${srcdir}"/idea-android android
  mv "${srcdir}"/idea-adt-tools-base android/tools-base

  # The class src/com/intellij/openapi/projectRoots/ex/JavaSdkUtil.java:56 (git commit 0ea5972cdad569407078fb27070c80e2b9235c53)
  # assumes the user's maven repo is at {$HOME}/.m2/repository and it contains junit-3.8.1.jar
  mkdir -p /build/.m2/repository/junit/junit/3.8.1/
  curl https://repo1.maven.org/maven2/junit/junit/3.8.1/junit-3.8.1.jar -o /build/.m2/repository/junit/junit/3.8.1/junit-3.8.1.jar

  echo ${_build} > build.txt
}

build() {
  cd intellij-community
  
  export JAVA_HOME="/usr/lib/jvm/java-${_jdkver}-openjdk"
  export PATH="/usr/lib/jvm/java-${_jdkver}-openjdk/bin:$PATH"
  export MAVEN_REPOSITORY=/build/.m2/repository
  
  ./installers.cmd -Dintellij.build.use.compiled.classes=false -Dintellij.build.target.os=linux
  tar -xf out/idea-ce/artifacts/ideaIC-${_build}-no-jbr.tar.gz -C "${srcdir}"
}

package() {
  cd idea-IC-${_build}

  install -dm 755 "${pkgdir}"/usr/share/{licenses,pixmaps,idea,icons/hicolor/scalable/apps}
  cp -dr --no-preserve='ownership' bin lib plugins "${pkgdir}"/usr/share/idea/
  cp -dr --no-preserve='ownership' license "${pkgdir}"/usr/share/licenses/idea
  ln -s /usr/share/idea/bin/idea.png "${pkgdir}"/usr/share/pixmaps/
  ln -s /usr/share/idea/bin/idea.svg "${pkgdir}"/usr/share/icons/hicolor/scalable/apps/
  install -Dm 644 ../idea.desktop -t "${pkgdir}"/usr/share/applications/
  install -Dm 755 ../idea.sh "${pkgdir}"/usr/bin/idea
  install -Dm 644 build.txt -t "${pkgdir}"/usr/share/idea
}

# vim: ts=2 sw=2 et:
