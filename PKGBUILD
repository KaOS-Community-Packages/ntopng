pkgname=ntopng
pkgver=3.8.1
_ndpiver=3.0
pkgrel=1
pkgdesc='The next generation version of the original ntop, a network traffic probe that shows the network usage'
arch=('x86_64')
url='http://www.ntop.org/'
license=('GPL3')
depends=('redis' 'geoip' 'mariadb' 'libpcap' 'sqlite' 'libmaxminddb' 'zeromq' 'lua' 'rrdtool' 'curl')
makedepends=('glib2' 'automake' 'libtool' 'wget' 'curl' 'libxml2')
install=$pkgname.install
source=("$pkgname-$pkgver.tar.gz::https://github.com/ntop/$pkgname/archive/$pkgver.tar.gz"
	"nDPI-$_ndpiver.tar.gz::https://github.com/ntop/nDPI/archive/$_ndpiver.tar.gz"
	"$pkgname@.service"
	"$pkgname.install"
	"$pkgname.sysusers")
sha512sums=('4cb613881e36b435f97dcc25dfaa505d9c552f63ad8334d5c9d47ec06376d47124c65e6d782546d36f6d27b34b037cc73df7129f459f290ac51e270bb7453970'
'74c4a41201e809b476f4c23b99c2391b7bcbc76507a11261d216caf2350db8fd4ae3dac69d1d2179b12217901da1e04676aeca05d3a8e63d1a162469b33ab4c0'
'217aae511a807f6d492fb074b81e417039226c42a85bbd5a5f776c11f72841931cb2ff1ed9d6463a495659c9bc9e278c5ed736d25772077c35bb259e8274a043'
'72749fbade0f14345481815355f42552bac3a9d053d86cf62caecd3cf7d304fadafd7ccf9a6a456ab7e5b8b0d36a3b92b1f4e1ecd0c40fa3d11e4a7d98cab39e'
'bb7f81a43e6bd1d58e41693dca1b5f03e507fb040bf036a5847a273f55bcfa665e8512220a54495c2926afb64e786d4e666556d7880be432cc7660de105e3ee4')

build() {
  cd $srcdir/nDPI-$_ndpiver
  ./autogen.sh
  ./configure
  make
  export NDPI_HOME=$srcdir/nDPI-$_ndpiver
  cd $srcdir/$pkgname-$pkgver
  ./autogen.sh
  ./configure --prefix=$pkgdir/usr --datadir=/usr/share
  make geoip
  make
}

package() {
  cd $srcdir/$pkgname-$pkgver

  make install

  mv $pkgdir/usr/{man,share/}
  install -Dm644 "$srcdir/$pkgname@.service" "$pkgdir/usr/lib/systemd/system/$pkgname@.service"
  install -Dm644 "$srcdir/$pkgname.sysusers" "$pkgdir/usr/lib/sysusers.d/$pkgname.conf"
}
