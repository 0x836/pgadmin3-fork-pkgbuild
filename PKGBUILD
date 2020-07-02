pkgname=pgadmin3-fork
gitpgkname=pgadmin3
pkgver=1.22.2
pkgrel=1
pkgdesc='Comprehensive design and management interface for PostgreSQL (legacy)'
url='https://www.pgadmin.org'
arch=('x86_64')
license=('custom')
depends=('wxgtk2' 'postgresql-libs' 'libxslt' 'libgcrypt')
makedepends=('libpqxx' 'krb5' 'postgresql' 'imagemagick')
source=('git+https://github.com/AbdulYadi/pgadmin3.git'
        pgadmin3-fix-segfault.patch)
md5sums=('SKIP'
	'SKIP')

prepare() {
  cd ${gitpgkname}
  convert +set date:create +set date:modify pgadmin/include/images/pgAdmin3.ico pgAdmin3.png
  # Fix segfault at startup (Debian)
  patch -p1 -i ../pgadmin3-fix-segfault.patch
  sed -E 's|(Categories=.+)|\1Database;|' -i pkg/pgadmin3.desktop
}

build() {
  export LANG=en_US.UTF-8
  export LC_ALL=en_US.UTF-8
  export PGADMIN_PYTHON_DIR=/usr

  cd ${gitpgkname}
  bash bootstrap
  ./configure \
    --prefix=/usr \
    --with-wx-version=3.0 \
    --with-libgcrypt \
    CFLAGS=-fPIC CXXFLAGS='-fPIC -Wno-narrowing'
  make
}

package() {
  cd ${gitpgkname}

  make DESTDIR="${pkgdir}" install
  install -Dm 644 i18n/${gitpgkname}.lng "${pkgdir}/usr/share/pgadmin3/i18n"
  install -Dm 644 LICENSE -t "${pkgdir}/usr/share/licenses/${pkgname}"

  install -Dm 644 pgadmin/include/images/pgAdmin3.ico -t "${pkgdir}/usr/share/pgadmin3"
  install -Dm 644 pgAdmin3-1.png "${pkgdir}/usr/share/pgadmin3/pgAdmin3.png"
  install -Dm 644 pgAdmin3-3.png "${pkgdir}/usr/share/icons/hicolor/16x16/apps/pgAdmin3.png"
  install -Dm 644 pgAdmin3-2.png "${pkgdir}/usr/share/icons/hicolor/32x32/apps/pgAdmin3.png"
  install -Dm 644 pgAdmin3-1.png "${pkgdir}/usr/share/icons/hicolor/48x48/apps/pgAdmin3.png"

  install -Dm 644 pkg/pgadmin3.desktop -t "${pkgdir}/usr/share/applications"
}
