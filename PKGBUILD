# Maintainer: Mark Wagie <mark dot wagie at proton dot me>
# Contributor: Christian Rebischke <chris.rebischke@archlinux.org>
# Contributor: Jeff Henson <jeff@henson.io>
# Contributor: Ron Asimi <ron dot asimi at gmail dot com>
# Contributor: Roman Perepelitsa <roman.perepelitsa@gmail.com>
pkgname=zsh-theme-powerlevel10k
# Whenever pkgver is updated, _libgit2ver below must also be updated.
pkgver=1.20.15  ## see P9K_VERSION in internal/p10k.zsh
pkgrel=2
pkgdesc="Powerlevel10k is a theme for Zsh. It emphasizes speed, flexibility and out-of-the-box experience."
arch=('x86_64' 'aarch64')
url='https://github.com/romkatv/powerlevel10k'
license=('MIT')
makedepends=(
  'git'
  'zig'
)
depends=(
  'zsh'
)
optdepends=(
  # It works well with Nerd Fonts, Source Code Pro, Font Awesome, Powerline,
  # and even the default system fonts. The full choice of style options is
  # available only when using Nerd Fonts.
  'ttf-meslo-nerd-font-powerlevel10k: recommended font'
  'powerline-fonts: patched fonts for powerline'
  'ttf-font-nerd: full choice of style options'
)
replaces=('zsh-theme-powerlevel9k')

# _libgit2ver depends on pkgver. They must be updated together. See libgit2_version in:
# https://raw.githubusercontent.com/romkatv/powerlevel10k/v${pkgver}/gitstatus/build.info
source=(
  "https://github.com/romkatv/powerlevel10k/archive/36f3045d69d1ba402db09d09eb12b42eebe0fa3b.tar.gz"
  "https://github.com/zig-pkgs/gitstatus/releases/download/v1.5.5/gitstatus-cache-v1.5.5.tar.gz"
  "https://github.com/zig-pkgs/gitstatus/archive/refs/tags/v1.5.5.tar.gz")
sha256sums=('61a0ffd025d75457fa07f10805b12479522b6ab68c6733c816012377fc23e1ff'
            '875c4662d0eb289f7439c96c1dd54ce47d297c12d8e5109a407fba32686c6c6e'
            'b79eb652e85607c86ff4ad626088d74d38f5321930e5dcbe95ef8d0ebe3e8175')
options=('!strip' '!debug')
#validpgpkeys=('8B060F8B9EB395614A669F2A90ACE942EB90C3DD') # Roman Perepelitsa <roman.perepelitsa@gmail.com>

export CARCH=aarch64

build() {
  export ZIG_GLOBAL_CACHE_DIR=$srcdir/gitstatus-cache-v1.5.5
  cd $srcdir/gitstatus-1.5.5
  zig build -Dtarget=aarch64-linux -Doptimize=ReleaseFast
}

package() {
  export ZIG_GLOBAL_CACHE_DIR=$srcdir/gitstatus-cache-v1.5.5
  cd $srcdir/gitstatus-1.5.5
  zig build -Dtarget=aarch64-linux -Doptimize=ReleaseFast \
    -p $pkgdir/usr/share/zsh-theme-powerlevel10k/gitstatus \
    --prefix-exe-dir usrbin

  cd $srcdir/powerlevel10k-36f3045d69d1ba402db09d09eb12b42eebe0fa3b
  find . -type f -exec install -D '{}' "$pkgdir/usr/share/${pkgname}/{}" ';'

  install -d "${pkgdir}/usr/share/licenses/${pkgname}"
  ln -s "/usr/share/${pkgname}/LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}"

  # delete unnecessary files. See also: https://bugs.archlinux.org/task/66737
  rm -rf "${pkgdir}/usr/share/${pkgname}/gitstatus/obj"
  rm -rf "${pkgdir}/usr/share/${pkgname}/gitstatus/.gitignore"
  rm -rf "${pkgdir}/usr/share/${pkgname}/gitstatus/.gitattributes"
  rm -rf "${pkgdir}/usr/share/${pkgname}/gitstatus/src"
  rm -rf "${pkgdir}/usr/share/${pkgname}/gitstatus/build"
  rm -rf "${pkgdir}/usr/share/${pkgname}/gitstatus/deps"
  rm -rf "${pkgdir}/usr/share/${pkgname}/gitstatus/Makefile"
  rm -rf "${pkgdir}/usr/share/${pkgname}/gitstatus/mbuild"
  rm "${pkgdir}/usr/share/${pkgname}/.gitattributes"
  rm "${pkgdir}/usr/share/${pkgname}/.gitignore"
  rm -rf "${pkgdir}/usr/share/${pkgname}/gitstatus/usrbin/.gitkeep"
  rm "${pkgdir}/usr/share/${pkgname}/gitstatus/.clang-format"
  rm -rf "${pkgdir}/usr/share/${pkgname}/gitstatus/.vscode/"

  cd "${pkgdir}/usr/share/${pkgname}"
  for file in *.zsh-theme internal/*.zsh gitstatus/*.zsh gitstatus/install; do
    zsh -fc "emulate zsh -o no_aliases && zcompile -R -- $file.zwc $file"
  done
}
