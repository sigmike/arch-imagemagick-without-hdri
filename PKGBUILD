# $Id: PKGBUILD 195181 2013-09-26 23:04:47Z eric $
# Maintainer: Eric Bélanger <eric@archlinux.org>

pkgbase=imagemagick
pkgname=('imagemagick' 'imagemagick-doc')
pkgver=6.8.7.0
pkgrel=1
arch=('i686' 'x86_64')
url="http://www.imagemagick.org/"
license=('custom')
makedepends=('libltdl' 'lcms2' 'libxt' 'fontconfig' 'libxext' 'ghostscript' \
             'openexr' 'libwmf' 'librsvg' 'libxml2' 'jasper' 'liblqr' \
             'opencl-headers' 'libcl' 'libwebp')
#source=(http://www.imagemagick.org/download/ImageMagick-${pkgver%.*}-${pkgver##*.}.tar.xz{,.asc} \
source=(ftp://ftp.sunet.se/pub/multimedia/graphics/ImageMagick/ImageMagick-${pkgver%.*}-${pkgver##*.}.tar.xz{,.asc} \
        perlmagick.rpath.patch)
sha1sums=('e7f5bc0cb03a16fb1b7c278c63bf4ac81e1642b2'
          'SKIP'
          'e143cf9d530fabf3b58023899b5cc544ba93daec')

prepare() {
  cd ImageMagick-${pkgver%.*}-${pkgver##*.}
  sed '/AC_PATH_XTRA/d' -i configure.ac
  autoreconf --force --install
  patch -p0 -i "${srcdir}/perlmagick.rpath.patch"
}

build() {
  cd ImageMagick-${pkgver%.*}-${pkgver##*.}
  ./configure --prefix=/usr --sysconfdir=/etc --with-modules --disable-static \
    --enable-hdri --with-wmf --with-openexr --with-xml --with-lcms2 --with-jp2 \
    --with-webp --with-gslib --with-gs-font-dir=/usr/share/fonts/Type1 \
    --with-perl --with-perl-options="INSTALLDIRS=vendor" --with-lqr --with-rsvg \
    --enable-opencl --without-gvc --without-djvu --without-autotrace \
    --without-jbig --without-fpx --without-dps --without-fftw 
 make
}

check() {
  cd ImageMagick-${pkgver%.*}-${pkgver##*.}
#  make check
}

package_imagemagick() {
  pkgdesc="An image viewing/manipulation program"
  depends=('perl' 'libltdl' 'lcms2' 'libxt' 'fontconfig' 'libxext' 'liblqr' 'libcl')
  optdepends=('ghostscript: for Ghostscript support' 
              'openexr: for OpenEXR support' 
              'libwmf: for WMF support' 
              'librsvg: for SVG support' 
              'libxml2: for XML support' 
              'jasper: for JPEG-2000 support' 
              'libpng: for PNG support' 
	      'libwebp: for WEBP support')
  backup=("etc/ImageMagick-${pkgver%%.*}/coder.xml"
          "etc/ImageMagick-${pkgver%%.*}/colors.xml"
          "etc/ImageMagick-${pkgver%%.*}/delegates.xml"
          "etc/ImageMagick-${pkgver%%.*}/log.xml"
          "etc/ImageMagick-${pkgver%%.*}/magic.xml"
          "etc/ImageMagick-${pkgver%%.*}/mime.xml"
          "etc/ImageMagick-${pkgver%%.*}/policy.xml"
          "etc/ImageMagick-${pkgver%%.*}/quantization-table.xml"
          "etc/ImageMagick-${pkgver%%.*}/thresholds.xml"
          "etc/ImageMagick-${pkgver%%.*}/type.xml"
          "etc/ImageMagick-${pkgver%%.*}/type-dejavu.xml"
          "etc/ImageMagick-${pkgver%%.*}/type-ghostscript.xml"
          "etc/ImageMagick-${pkgver%%.*}/type-windows.xml")
  options=('!docs' 'libtool' '!emptydirs')

  cd ImageMagick-${pkgver%.*}-${pkgver##*.}
  make -j1 DESTDIR="${pkgdir}" install
  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/imagemagick/LICENSE"
  install -Dm644 NOTICE "${pkgdir}/usr/share/licenses/imagemagick/NOTICE"

#Cleaning
  rm -f "${pkgdir}"/usr/lib/*.la
}

package_imagemagick-doc() {
  pkgdesc="The ImageMagick documentation (utilities manuals and libraries API)"

  cd ImageMagick-${pkgver%.*}-${pkgver##*.}
  make DESTDIR="${pkgdir}" install-data-html
  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/imagemagick-doc/LICENSE"
  install -Dm644 NOTICE "${pkgdir}/usr/share/licenses/imagemagick-doc/NOTICE"
}
