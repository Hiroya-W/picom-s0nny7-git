pkgname=picom-s0nny7-git
_gitname=picom
pkgver=1578_Next.297.gd74c099_2021.06.09
pkgrel=1
pkgdesc="s0nny7's compton|picom fork (X compositor) Fixed shadow issue"
arch=(i686 x86_64)
url="https://github.com/s0nny7/${_gitname}"
license=('MIT' 'MPL2')
depends=('libgl' 'libev' 'pcre' 'libx11' 'xcb-util-renderutil' 'libxcb' 'xcb-util-image' 'libxext'
         'pixman' 'libconfig' 'libdbus' 'hicolor-icon-theme' 'libxdg-basedir')
makedepends=('git' 'mesa' 'meson' 'asciidoc' 'uthash')
optdepends=('dbus:          To control picom via D-Bus'
            'xorg-xwininfo: For picom-trans'
            'xorg-xprop:    For picom-trans'
            'python:        For picom-convgen.py')
provides=('compton' 'compton-git' 'picom' 'picom-git')
conflicts=('compton' 'compton-git' 'picom' 'picom-git')
#replaces=('compton-git' 'picom-git')
options=('!strip')
source=(git+"https://github.com/s0nny7/${_gitname}.git#branch=fix-shadow-clean")
md5sums=("SKIP")

pkgver() {
    cd ${_gitname}
    _tag=$(git describe --tags | sed 's:^v::') # tag is mobile, and switches between numbers and letters, can't use it for versioning
    _commits=$(git rev-list --count HEAD) # total commits is the most sane way of getting incremental pkgver
    _date=$(git log -1 --date=short --pretty=format:%cd)
    printf "%s_%s_%s\n" "${_commits}" "${_tag}" "${_date}" | sed 's/-/./g'
}

build() {
  cd "${srcdir}/${_gitname}"
  meson --buildtype=release . build --prefix=/usr -Dwith_docs=true
  ninja -C build
}

package() {
  cd "${srcdir}/${_gitname}"

  DESTDIR="${pkgdir}" ninja -C build install

  # install license
  install -D -m644 "LICENSES/MIT" "${pkgdir}/usr/share/licenses/${pkgname/-git$/}/LICENSE-MIT"

  # example conf
  install -D -m644 "picom.sample.conf" "${pkgdir}/etc/xdg/picom.conf.example"
}
