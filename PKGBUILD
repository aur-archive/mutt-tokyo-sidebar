# Maintainer: Nick Hu <me@nickhu.co.uk>
# Contributor: Alessio Sergi <asergi at archlinux dot us>
# Contributor: tobias [tobias [at] archlinux.org]
# Contributor: Gaetan Bisson <bisson@archlinux.org>

pkgname=mutt-tokyo-sidebar
pkgver=1.5.23
pkgrel=1
pkgdesc='Small but very powerful text-based mail client with tokyocabinet and sidebar-patch'
arch=('i686' 'x86_64')
url='http://www.mutt.org/'
license=('GPL')
depends=('gpgme' 'tokyocabinet' 'slang' 'mime-types')
optdepends=('smtp-forwarder: to send mail')
conflicts=('mutt')
provides=('mutt')
source=("https://bitbucket.org/mutt/mutt/downloads/mutt-${pkgver}.tar.gz"
        "http://lunar-linux.org/~tchan/mutt/patch-1.5.23.sidebar.20140412.txt"
        "ftp://ftp.openbsd.org/pub/OpenBSD/distfiles/mutt/trashfolder-1.5.22.diff0.gz"
        "patch-for-first-char-jf.patch")
sha512sums=('f1b4a7230253651857f61bd7215cce870a613012f613d4c907d401556083726c8ed7d429d57a8bf858c3b5b23683380d4c1494540d86ca80813e22cb6b95bc1e'
            'ac023f1ba5843a010de497b9cd85b22af6a30ceb8622c01be3c5b7e8e0761d8efe8e86e8c9b11aa96dcc405e8b1657672ec6da111b703d562bddf35ba848d34d'
            'a00075888c12ce55e5173788d9e0397f4cc65f8344328112342399b9734f041e80cceaee18ce68fe79471457e72b743b08112bf4f71d251db25f45ed5ee27ada'
            '16ba918a8d5ef091cd7556e97189b43a0afdc1fc23a8bb9ece6a97281223090a5e680886233d2aa52e5340c402d19c68801b51eec13ae61cd8d301418470f11d')

prepare() {
  cd "$srcdir/mutt-$pkgver"

  # patch to add trash folder support
  patch -Np1 -i "$srcdir"/trashfolder-1.5.22.diff0

  # patch to add sidebar support
  patch -Np1 -i "$srcdir"/patch-1.5.23.sidebar.20140412.txt

  # patch for first char in mailboxdir
  patch -i "$srcdir"/patch-for-first-char-jf.patch

  # fix automake issue
  autoreconf -vfi
}

build() {
  cd "$srcdir/mutt-$pkgver"
  ./configure --prefix=/usr --sysconfdir=/etc \
  --disable-pop --disable-imap --disable-smtp \
  --with-curses=/usr \
  --enable-gpgme --enable-hcache \
  --without-ssl --without-sasl --without-idn \
  --with-tokyocabinet --without-qdbm --without-gdbm \
  --with-regex --with-slang

  make
}

package() {
  cd "$srcdir/mutt-$pkgver"
  make DESTDIR="$pkgdir" install

  # remove unneeded or conflicting files
  rm "$pkgdir"/etc/mime.types{,.dist}
  rm "$pkgdir"/usr/bin/{flea,muttbug}
  rm "$pkgdir"/usr/share/man/man1/{flea,muttbug}.1

  # install Muttrc.gpg file
  install -D -m 644 contrib/gpg.rc "$pkgdir"/etc/Muttrc.gpg.dist
}
