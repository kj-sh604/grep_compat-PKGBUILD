# Maintainer: kj_sh604 
# don't talk to me 

pkgname=grep_compat
pkgname_=grep
pkgver=3.11
pkgrel=1
pkgdesc='A string search utility'
arch=('x86_64')
license=('GPL3')
url='https://www.gnu.org/software/grep/'
depends=('glibc' 'pcre2')
makedepends=('texinfo')
provides=('grep')
conflicts=('grep')
source=("https://ftp.gnu.org/gnu/$pkgname_/$pkgname_-$pkgver.tar.xz"{,.sig})

prepare() {
  cd $pkgname_-$pkgver
  # apply patch from the source array (should be a pacman feature)
  local src
  for src in "${source[@]}"; do
    src="${src%%::*}"
    src="${src##*/}"
    [[ $src = *.patch ]] || continue
    msg2 "Applying patch $src..."
    patch -Np1 < "../$src"
  done
}

build() {
  cd $pkgname_-$pkgver
  ./configure --prefix=/usr
  make
}

check() {
  cd $pkgname_-$pkgver
  make check
}

package() {
  cd $pkgname_-$pkgver
  make DESTDIR="$pkgdir" install

  # add egrep wrapper script with no warnings
  install -Dm755 /dev/stdin "$pkgdir/usr/bin/egrep" <<END
#!/bin/sh
cmd=\${0##*/}
exec grep -E "\$@"
END

  # add fgrep wrapper script with no warnings
  install -Dm755 /dev/stdin "$pkgdir/usr/bin/fgrep" <<END
#!/bin/sh
cmd=\${0##*/}
exec grep -F "\$@"
END
}
