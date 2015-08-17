# $Id: PKGBUILD 50698 2011-06-29 15:35:12Z stephane $
# Maintainer: Sergej Pupykin <pupykin.s+arch@gmail.com>

pkgname=wml
pkgver=2.0.11
pkgrel=7
pkgdesc="The Website Meta Language"
arch=('i686' 'x86_64')
url="http://thewml.org/"
license=('GPL')
depends=('perl' 'libpng' 'gdbm' 'db' 'ncurses')
makedepends=('lynx')
source=("http://thewml.org/distrib/${pkgname}-${pkgver}.tar.gz")
md5sums=('a26feebf4e59e9a6940f54c69dde05b5')
build() {
  cd ${pkgname}-${pkgver}

  # missing Perl modules fix
  sed -i 's/PREFIX=$(libdir)\/perl/DESTDIR=\.\.\/\.\.\/\.\.\/\.\.\/pkg\/ PREFIX=$(libdir)\/perl/' wml_common/Makefile.in
  sed -i 's/$(MAKE) pure_perl_install $(MM_INSTALL_OPTS)/$(MAKE) pure_perl_install/' wml_common/Makefile.in

  unset LDFLAGS
  ./configure \
    --prefix=/usr

  # compile fixhack
  sed -i 's#/usr/lib/perl5/core_perl/auto/DynaLoader/DynaLoader.a##' wml_backend/p3_eperl/Makefile
  sed -i 's/extern struct option options\[\]\;//' ${srcdir}/${pkgname}-${pkgver}/wml_backend/p3_eperl/eperl_proto.h
  sed -i 's|strip $dsttmp|#strip $dsttmp|' etc/shtool
  mkdir -p ${pkgdir}/usr/bin ${pkgdir}/usr/lib/wml/exec ${pkgdir}/usr/man/man{1,3,7} ${pkgdir}/usr/man/cat{1,7}

#  make clean
  make
}

package() {
  cd ${pkgname}-${pkgver}

  make prefix=${pkgdir}/usr install

  [ -d ${pkgdir}/usr/man ] && mkdir -p ${pkgdir}/usr/share && mv ${pkgdir}/usr/man ${pkgdir}/usr/share
}
