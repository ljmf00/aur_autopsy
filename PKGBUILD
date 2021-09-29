# Maintainer: Oscar Shrimpton <oscar.shrimpton.personal@gmail.com>
pkgname=autopsy
pkgver=4.18.0
pkgrel=1
pkgdesc='Digital forensics platform and graphical interface to The Sleuth Kit® and other digital forensic tools'
arch=(x86_64)
url='http://www.sleuthkit.org/autopsy/'
license=('Apache-2.0')
_skver=4.10.2
depends=(java-runtime=8 testdisk sleuthkit "sleuthkit-java=${_skver}" java8-openjfx)
makedepends=('git' 'ant')
optdepends=('opencv: media files (64-bit)'
			'perl-parse-registry: regripper')
source=("${pkgname}-${pkgver}::git+https://github.com/sleuthkit/autopsy.git#tag=${pkgname}-${pkgver}"
		"sleuthkit-${_skver}::git+https://github.com/sleuthkit/sleuthkit.git#tag=sleuthkit-${_skver}"
		Autopsy.desktop)
sha512sums=('SKIP'
	    'SKIP'
            'd209bc1947eccaee7520243e8f8ce81daa71404e547a671eec7ca9e95572e305ae75db8058784631ac721c7612de405347be2b4d3124f324c06af9efb6dd82a7')
prepare() {
	cd "${pkgname}-${pkgver}"

	sed -i 's/netbeans-vm.apache.org/netbeans-vm1.apache.org/g' nbproject/platform.properties
}

build() {
	cd "$srcdir/sleuthkit-${_skver}"

	./bootstrap
	./configure --prefix=/usr
	make

	pushd bindings/java
		ant -q dist
	popd

	cd "$srcdir/${pkgname}-${pkgver}"

	export TSK_HOME="$srcdir/sleuthkit-${_skver}"
	ant -v -q build
}

package() {
  cd "${pkgname}-${pkgver}"

  mkdir -p $pkgdir/usr/share/${pkgname}
  cp -r * $pkgdir/usr/share/${pkgname}/

  # copy sleuthkit jar into autopsy
  mkdir -p "$pkgdir/usr/share/${pkgname}/${pkgname}/modules/ext/"
  rm -f "$pkgdir/usr/share/${pkgname}/${pkgname}/modules/ext/sleuthkit-${_skver}.jar"
  ln -s /usr/share/java/sleuthkit-${_skver}.jar $pkgdir/usr/share/${pkgname}/${pkgname}/modules/ext/sleuthkit-${_skver}.jar

  # overwrite bin/autopsy with proper permissions
  install -m755 bin/autopsy $pkgdir/usr/share/${pkgname}/bin/autopsy

  # symlink to executable
  mkdir -p $pkgdir/usr/bin
  ln -sr $pkgdir/usr/share/${pkgname}/bin/autopsy $pkgdir/usr/bin/autopsy

  mkdir -p $pkgdir/usr/share/pixmaps
  cp icon.ico $pkgdir/usr/share/pixmaps/autopsy.ico

  mkdir -p $pkgdir/usr/share/applications
  install -Dm644 $srcdir/Autopsy.desktop $pkgdir/usr/share/applications
}
