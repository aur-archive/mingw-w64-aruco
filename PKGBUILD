pkgname=mingw-w64-aruco
pkgver=1.2.5
pkgrel=4
pkgdesc="A minimal library for Augmented Reality applications based on OpenCV"
url="http://www.uco.es/investiga/grupos/ava/node/26" 
arch=('any')
license=('BSD')
options=('!buildflags' 'staticlibs' '!strip')
depends=('mingw-w64-opencv>=2.1' 'mingw-w64-freeglut')
makedepends=('mingw-w64-cmake' 'mingw-w64-gcc')
source=("http://sourceforge.net/projects/aruco/files/1.2.5/aruco-1.2.5.tgz/download"
        "aruco-${pkgver}-mingw.patch"
        "aruco-${pkgver}-chromaticmask.patch")
md5sums=('5da7b626c7f78ba4ac09debf70bf5f55'
         '8dcca23290c08aeb7f2e976ab7c28dd4'
         '8e30f8bbb3691dae40f1bee0b5a1a8c6')
_architectures="i686-w64-mingw32 x86_64-w64-mingw32"

prepare() {
  cd "${srcdir}/aruco-${pkgver}"
  patch -p1 -i ../aruco-${pkgver}-mingw.patch
  patch -p1 -i ../aruco-${pkgver}-chromaticmask.patch
}

build() {
  cd "${srcdir}/aruco-${pkgver}"
  for _arch in ${_architectures}; do
    mkdir -p build-${_arch} && pushd build-${_arch}
    ${_arch}-cmake \
      -DCMAKE_SKIP_RPATH=ON \
      -DCMAKE_BUILD_TYPE=Release \
      ..
    make
    popd
  done
  for _arch in ${_architectures}; do
    mkdir -p build-${_arch}-static && pushd build-${_arch}-static
    ${_arch}-cmake \
      -DCMAKE_SKIP_RPATH=ON \
      -DCMAKE_BUILD_TYPE=Release \
      -DBUILD_SHARED_LIBS=OFF \
      ..
    make
    popd
  done
}

package() {
  for _arch in ${_architectures}; do
    cd "${srcdir}/aruco-${pkgver}/build-${_arch}-static"
    make DESTDIR="$pkgdir" install
    cd "${srcdir}/aruco-${pkgver}/build-${_arch}"
    make DESTDIR="$pkgdir" install
    install -d "$pkgdir/usr/${_arch}/share/licenses/$pkgname"
    install -Dm644 "$srcdir"/aruco-${pkgver}/COPYING "$pkgdir/usr/${_arch}/share/licenses/$pkgname/COPYING"
    rm -r $pkgdir/usr/${_arch}/bin/*.exe
    ${_arch}-strip --strip-unneeded "$pkgdir"/usr/${_arch}/bin/*.dll
    ${_arch}-strip -g "$pkgdir"/usr/${_arch}/lib/*.a
  done
}

# vim:set ts=2 sw=2 et:
