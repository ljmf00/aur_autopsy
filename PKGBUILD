# Maintainer: Jaryl Chng <mrciku@gmail.com>
# Contributor: Olivier Bilodeau <obilodeau@gosecure.ca>
pkgname=autopsy
pkgver=4.7.0
pkgrel=2
pkgdesc='The Autopsy Forensic Browser is a GUI for The Sleuth Kit.'
arch=(x86_64)
url='http://www.sleuthkit.org/autopsy/'
license=('MIT/Apache-2.0')
provides=(autopsy)
depends=(java-runtime testdisk sleuthkit sleuthkit-java java-openjfx)
makedepends=()
source=(https://github.com/sleuthkit/${pkgname}/releases/download/${pkgname}-${pkgver}/${pkgname}-${pkgver}.zip Autopsy.desktop)
sha256sums=(de85415aef78236f6f135b6ab4376470e60c154f58b867d676bf5b11df40f766 be382bc92f5e98dfebbbf31dc927fc44af0fecee6911f7122ba8e7c55d281262)

package() {
  cd "${pkgname}-${pkgver}"

  mkdir -p $pkgdir/opt/${pkgname}
  cp -r * $pkgdir/opt/${pkgname}/

  # copy sleuthkit jar into autopsy
  cp /usr/share/java/sleuthkit-4.6.1.jar $pkgdir/opt/${pkgname}/${pkgname}/modules/ext/sleuthkit-postgresql-4.6.1.jar

  # overwrite bin/autopsy with proper permissions
  install -m755 bin/autopsy $pkgdir/opt/${pkgname}/bin/autopsy

  mkdir -p $pkgdir/usr/share/pixmaps
  cp icon.ico $pkgdir/usr/share/pixmaps/autopsy.ico

  mkdir -p $pkgdir/usr/share/applications
  install -Dm644 ../../Autopsy.desktop $pkgdir/usr/share/applications
}
