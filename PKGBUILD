pkgname=lbanet-physx
pkgver=2.8.3
pkgrel=1
pkgdesc="A large physics middleware library for game production"
arch=('i686' 'x86_64')
url="http://www.nvidia.com"
license=('custom')
makedepends=('binutils' 'tar')
depends=('freeglut')
conflicts=('physx')
options=(docs !strip)

if [ "$CARCH" = "x86_64" ]; then
	depends+=('gcc-libs-multilib')
	source=(https://neolbanet.googlecode.com/files/PhysX_2.8.3.3_for%20Linux_x64_Core_deb_tar.gz)
	md5sums=('1f00e894251d429ed16e9e04333bf365')
elif [ "$CARH" = "i686" ]; then
	source=(https://neolbanet.googlecode.com/files/PhysX_2.8.3.3_for_Linux_x32_Core_deb_tar.gz)
	md5sums=('4d36e1f948fb55a664177503eed6eeea')
fi

_extroot="root"
_debxtract() {
	msg "Extracting .deb package: $1"
	ar xvf "$1"
	if pushd "$_extroot" >/dev/null; then
		tar xvf ../data.tar.gz
		popd >/dev/null
	fi
}

build() {
	cd $srcdir
	rm -rf "$_extroot"
	mkdir -p "$_extroot"

	for i in libphysx-{,common-,dev-,extras-}2.8.3_3_amd64.deb; do
		_debxtract "$i"
	done
}

package() {
	cp -pr "$srcdir/$_extroot/." "$pkgdir"

	if pushd "$pkgdir/usr/lib/PhysX/v$pkgver"; then
		ln -s libPhysXCore.so.1 libPhysXCore.so
		ln -s libNxCooking.so.1 libNxCooking.so
		popd
	fi

	if pushd "$pkgdir/usr/lib"; then
		ln -s libPhysXLoader.so.1 libPhysXLoader.so
		popd
	fi

	mkdir -p "$pkgdir/usr/share/licenses/$pkgname"
	cp "$pkgdir/usr/share/doc/libphysx-common-$pkgver/copyright" "$pkgdir/usr/share/licenses/$pkgname/"
}
