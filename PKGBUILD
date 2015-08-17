# Maintainer: Jaime Machado <jaime.machado@gmail.com>
pkgname=openct-nopcsc
pkgver=0.6.20
pkgrel=5
pkgdesc="Implements drivers for several smart card readers (pcsc disabled)"
arch=('i686' 'x86_64')
url="https://github.com/OpenSC/openct/"
options=('!libtool')
license="LGPL"
backup=('etc/openct.conf')
provides=('openct')
conflicts=('openct')
depends=('libusb-compat' 'libtool')
source=("https://github.com/OpenSC/openct/archive/openct-$pkgver.tar.gz"
        'openct.rc'
        'udev-sleep.patch'
	'openct.service')

build() {
        cd "$srcdir/openct-openct-$pkgver"
        patch -p1 < $srcdir/udev-sleep.patch
        ./bootstrap
        ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var --with-udev=/usr/lib/udev --enable-usb --disable-pcsc --disable-static
        make
}

package() {
        cd "$srcdir/openct-openct-$pkgver"
        # Work around broken makefile
        mkdir $pkgdir/etc
        make DESTDIR="$pkgdir" install

        install -D etc/openct.udev $pkgdir/usr/lib/udev/rules.d/95-openct.rules
	install -D $srcdir/openct.service $pkgdir/usr/lib/systemd/system/openct.service
        install -D -m755 $srcdir/openct.rc $pkgdir/etc/rc.d/openct

        mkdir -p $pkgdir/var/run/openct
}
md5sums=('30e416c6c414466f685a5e2db8591c71'
         '000bab3e5a98e49159e8190e2b318c74'
         '1c8484195d3b8445ebdb9fdc2ee87736'
         'f18c1520e60dafedb0af874c1b13f6ce')
