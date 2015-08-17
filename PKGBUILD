# Maintainer: Christian Hammerl <info@christian-hammerl.de>

_pkgname=proftpd
pkgname=${_pkgname}-sqlquota
pkgver=1.3.3e
pkgrel=1
pkgdesc="A high-performance, scalable FTP server (with mod_quotatab_sql enabled)"
arch=('i686' 'x86_64')
url="http://www.proftpd.org"
license=('GPL')
conflicts=(proftpd)
provides=(proftpd)
depends=('glibc' 'pam' 'ncurses' 'libcap' 'libldap' 
	 'libmysqlclient' 'postgresql-libs')
backup=('etc/proftpd.conf' 'etc/conf.d/proftpd')
source=(ftp://ftp.proftpd.org/distrib/source/${_pkgname}-${pkgver}.tar.bz2
	'proftpd' 'proftpd.logrotate' 'proftpd.conf.d')
md5sums=('acc49b6589bc8c9fdf1dce9000bebdbd'
         '99f6f9a989e70e3fa50809fc2bbbbb0a'
         'ddb09eb13131becdf0e081eef413116b'
         '71d5932b0461c318ed68c2c0c2660736')
sha1sums=('b347aa72d12e41fe8f43e8d91a7a4eeaac6f472f'
          'b7819d725817e55b69c73e2572c21a05db48cc86'
          '83c38ec40efb7cc09d9824b98e65cd948a195cc6'
          'f34f60cb4fb1f4af7be7aca427cbad3cad22bbb9')

build() {
	cd "${srcdir}/${_pkgname}-${pkgver}"

	./configure --prefix=/usr --mandir=/usr/share/man --disable-pam \
	    --with-modules=mod_quotatab:mod_quotatab_file:mod_quotatab_sql:mod_tls:mod_ldap:mod_sql:mod_sql_mysql:mod_sql_postgres \
	    --sysconfdir=/etc --localstatedir=/var/run --enable-ctrls --enable-ipv6 \
	    --with-includes=/usr/include/mysql:/usr/include/postgresql \
	    --with-libraries=/usr/lib/mysql:/usr/lib/postgresql --enable-nls
	make
}

package() {
	cd "${srcdir}/${_pkgname}-${pkgver}"

	make DESTDIR="${pkgdir}" install
	install -Dm644 ../proftpd.logrotate "${pkgdir}/etc/logrotate.d/proftpd"
	install -Dm644 ../proftpd.conf.d "${pkgdir}/etc/conf.d/proftpd"
	install -Dm755 ../proftpd "${pkgdir}/etc/rc.d/proftpd"
	install -Dm755 contrib/xferstats.holger-preiss \
	   "${pkgdir}/usr/bin/ftpstats"
	cd "${pkgdir}/etc"
	sed -i 's|nogroup|nobody|g' proftpd.conf
	rmdir "${pkgdir}/usr/libexec"
}
