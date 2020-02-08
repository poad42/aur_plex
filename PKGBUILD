# Maintainer: Daniel Egeberg <daniel.egeberg@gmail.com>
# Contributor: Sven-Hendrik Haase <sh@lutzhaase.com>
# Contributor: TingPing <tingping@tingping.se>

pkgname=plex-media-player
pkgver=2.50.0
_gitrev=1045
_gitver=37e9e857
_fullver="$pkgver.$_gitrev-$_gitver"
_fullname="$pkgname-$_fullver"
_web_buildid="166-78faf811b12c68"
_web_desktop_ver="3.104.2-1b12c68"
_web_tv_ver="4.17.1-65a9063"
pkgrel=3
pkgdesc='Next generation Plex Desktop Client'
arch=('i686' 'x86_64' 'armv7h')
license=('GPL')
url='https://github.com/plexinc/plex-media-player'
depends=('mpv' 'qt5-webengine-595' 'libcec' 'sdl2' 'qt5-x11extras-595' 'qt5-quickcontrols-595' 'p8-platform' 'protobuf')
makedepends=('cmake')
source=("$_fullname.tar.gz::https://github.com/plexinc/plex-media-player/archive/v${_fullver}.tar.gz"
        "buildid-${_web_buildid}.cmake::https://artifacts.plex.tv/web-client-pmp/${_web_buildid}/buildid.cmake"
        "web-client-desktop-${_web_buildid}-${_web_desktop_ver}.tar.xz::https://artifacts.plex.tv/web-client-pmp/${_web_buildid}/web-client-desktop-${_web_desktop_ver}.tar.xz"
        "web-client-desktop-${_web_buildid}-${_web_desktop_ver}.tar.xz.sha1::https://artifacts.plex.tv/web-client-pmp/${_web_buildid}/web-client-desktop-${_web_desktop_ver}.tar.xz.sha1"
        'qt.patch')
noextract=("web-client-desktop-${_web_buildid}-${_web_desktop_ver}.tar.xz")
sha512sums=('3ccbe5b71bb55c9aca5b457364812ee784cf1388f737d27dec82d1fc0fcc4dbe5caa437dea4ed8596c8dd36d73a7387eac8a621961aedfd65627c30bdeb9e410'
            'fc97a14cde5a43985d0e58f580cb1fb90df3f199bda505016cebbe73f76964ef16cadea70374d36ac740817a6e99445ae7c915ae552cae74c515bd7bc87fe2c5'
            '66a37f9288837efe15859bf97772894181ee9807f6fa29b88320b675ea97f1d4557ab39b652cd83f5d94dcdc4bf22b7efdc6ef9b9ee4ad39b009f7f4670fc32d'
            '7efd3858a3f37eb756b5a0713ea68329ca510f9cc4a3a90a9529da778cc4c7aa71568045bf5d2e9cb6aaed137100b36d32878abea84a7e47c42c349a0a8f3df4'
            'SKIP')

prepare() {
    cd "${srcdir}/$_fullname"

    patch --forward --strip=1 --input="${srcdir}/qt.patch"

    # All this git version junk fails, just remove it we already have the version
    sed -i 's|include(GetGitRevisionDescription)||
            s|get_git_head_revision(REFSPEC FULL_GIT_REVISION)||' \
           CMakeModules/VersionConfiguration.cmake

    mkdir -p build/dependencies
    for f in "buildid-${_web_buildid}.cmake"; do
        ln -sf "${srcdir}/${f}" "build/dependencies/${f}"
    done
    for f in "web-client-desktop-${_web_buildid}-${_web_desktop_ver}.tar.xz"{,.sha1} ; do
        target="${f/-${_web_buildid}-/-}"
        ln -sf "${srcdir}/${f}" "build/dependencies/${target}"
    done
}

build() {
    QT_BASE_DIR=/opt/qt595
    export QTDIR=$QT_BASE_DIR
    export PATH=$QT_BASE_DIR/bin:$PATH
    cd "${srcdir}/$_fullname/build"

    cmake -DCMAKE_INSTALL_PREFIX='/usr' -DCMAKE_BUILD_TYPE='Release' \
          -DFULL_GIT_REVISION="$_gitver" -DQTROOT='/opt/qt595' \
          ..
    make
}

package() {
    cd "${srcdir}/$_fullname/build"

    DESTDIR="$pkgdir" make install
}
