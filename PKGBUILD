# Maintainer: Richard "Nothing4You" Schwab <mail NOSPAM w.tf-w.tf>

# Found typos, bugs or other problems? Feel free to create a pull-request:
# https://github.com/Nothing4You/PKGBUILDs

pkgname=ffmpeg-full-git
_pkgname=ffmpeg
pkgver=0.r64737.8e29768
pkgrel=1
pkgdesc="Record, convert, and stream audio and video (all codecs; git version)"
arch=('i686' 'x86_64')
url="http://ffmpeg.org/"
depends=('celt'
         'faac'
         'gnutls'
         'gsm'
         'jack'
         'lame'
         'libass'
         'libavc1394'
         'libbluray'
         'libcaca'
         'libcdio-paranoia'
         'libdc1394'
         'libfdk-aac'
         'libiec61883'
         'libmodplug'
#         'libnut-git' # does not build
         'libpulse'
         'libquvi-0.4' # does not build with normal libquvi
         'libsoxr-git'
         'libssh'
         'libtheora'
         'libva'
         'libvpx'
         'openal'
         'opencore-amr'
         'openjpeg'
         'opus'
         'rtmpdump'
         'schroedinger'
         'sdl'
         'speex'
         'twolame'
         'utvideo-git'
         'v4l-utils'
         'vo-aacenc'
         'vo-amrwbenc'
         'x264'
         'xvidcore'
         'zeromq')
makedepends=('git' 'yasm' 'pkg-config')
conflicts=('ffmpeg' 'ffmpeg-git' 'ffmpeg-full')
provides=('ffmpeg' 'ffmpeg-git' 'qt-faststart')
license=('GPL' 'custom:UNREDISTRIBUTABLE')
source=('git://git.videolan.org/ffmpeg#branch=master'
        'UNREDISTRIBUTABLE.txt')
sha256sums=('SKIP'
            'e0c1b126862072a71e18b9580a6b01afc76a54aa6e642d2c413ba0ac9d3010c4')
sha512sums=('SKIP'
            '06b853eff2d43fe437d09649229cd7f7466c0ecc75b324027d68b4fe365e9722a8033b15f4075c2ad4a845c61d21bbe551edfb0efb2e8e0e08d205ebc402164d')

pkgver() {
  cd "$srcdir/$_pkgname"
  printf "0.r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
  cd "$srcdir/$_pkgname"

  msg "Starting configure..."

  ./configure \
    --prefix=/usr \
    --enable-debug \
    --enable-shared \
    --disable-static \
    \
    --enable-gpl \
    --enable-nonfree \
    --enable-version3 \
    \
    --enable-pic \
    \
    --enable-avresample \
    --enable-bzlib \
    --enable-fontconfig \
    --enable-gnutls \
    --enable-libass \
    --enable-libbluray \
    --enable-libcaca \
    --enable-libcdio \
    --enable-libcelt \
    --enable-libdc1394 \
    --enable-libiec61883 \
    --enable-libfaac \
    --enable-libfdk-aac \
    --enable-libfreetype \
    --enable-libgsm \
    --enable-libmp3lame \
    --enable-libmodplug \
    --enable-libopenjpeg \
    --enable-libopencore_amrnb \
    --enable-libopencore_amrwb \
    --enable-libopus \
    --enable-libpulse \
    --enable-librtmp \
    --enable-libschroedinger \
    --enable-libsoxr \
    --enable-libspeex \
    --enable-libtheora \
    --enable-libquvi \
    --enable-libssh \
    --enable-libtwolame \
    --enable-libutvideo \
    --enable-libv4l2 \
    --enable-libvorbis \
    --enable-libvo-aacenc \
    --enable-libvo-amrwbenc \
    --enable-libvpx \
    --enable-libx264 \
    --enable-libxvid \
    --enable-openal \
    --enable-openssl \
    --enable-vaapi \
    --enable-vda \
    --enable-vdpau \
    --enable-x11grab \
    --enable-libzmq \
    \

## dropped from configure flags in the ABS
#    --enable-dxva2 \
#    --enable-postproc \          # enabled by default
#    --enable-runtime-cpudetect \ # enabled by default
#    --enable-swresample \        # enabled by default

## from old PKGBUILD

#    --enable-libnut \ # ./configure does not find libnut, tested with libnut-git

  msg "Starting make"
  make
  make tools/qt-faststart
  make doc
}

package() {
  cd "$srcdir/$_pkgname"
  make DESTDIR="$pkgdir" install install-man
  install -D -m755 tools/qt-faststart "$pkgdir/usr/bin/qt-faststart"
  install -D -m644 "$srcdir/UNREDISTRIBUTABLE.txt" "$pkgdir/usr/share/licenses/$pkgname/UNREDISTRIBUTABLE.txt"
}

### leftover from old PKGBUILD, keeping this for reference for now

# How to audit the ./configure flags:
#
# cut -c 3- <<'# EOF' | sh
# cd src/ffmpeg
# export DISABLED='
# # debugging flags follow:
# --enable-coverage
# --enable-extra-warnings
# --enable-ftrapv
# --enable-memalign-hack
# --enable-memory-poisoning
# --enable-random
# --enable-xmm-clobber-test
# # we do not want this:
# --enable-cross-compile       # not cross building
# --enable-gray                # slow
# --enable-hardcoded-tables    # no advantage
# --enable-lto                 # slow build
# --enable-small               # we want SPEED instead
# # this stuff does not build:
# --enable-frei0r              # circular dependency
# --enable-libaacplus          # does not build from AUR: configure.ac:8: error: "AM_CONFIG_HEADER": this macro is obsolete.
# --enable-libflite            # configure fail: /usr/lib/gcc/x86_64-unknown-linux-gnu/4.7.2/../../../../lib/libflite.a(au_alsa.o): In function "audio_open_alsa": (.text+0x20): undefined reference to "snd_pcm_hw_params_sizeof"
# --enable-libilbc             # configure fail: /tmp/ffconf.lccg5Ux6.c:1:18: fatal error: ilbc.h: No such file or directory
# --enable-libopencv           # circular dependency
# --enable-libstagefright-h264 # not in AUR
# --enable-libxavs             # does not build from AUR: /usr/bin/ld: common/i386/deblock.o: relocation R_X86_64_32 against ".rodata" can not be used when making a shared object; recompile with -fPIC
# # this stuff is not for linux/x86:
# --enable-avisynth            # windows only
# --enable-dxva2               # windows only
# --enable-sram                # not x86
# --enable-thumb               # not x86
# '
# ./configure --help | perl -ne 'for(/--enable-([0-9a-z-]+)\s/) { if($ENV{DISABLED} !~ /^--enable-$_\b/m) { print "    --enable-$_ \\\n"; } }' | sort -u
# EOF
