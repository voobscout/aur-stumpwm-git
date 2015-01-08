# Maintainer: Voobscout <voobscout+aur@gmail.com>
# Original maintainer: M Rawash <mrawash@gmail.com>
# Contributor: olvar <beren dot olvar (at) gmail dot com>
# Contributor: Andrew Antle <andrew dot antle at gmail dot com>
# Contributor: joyfulgirl <joyfulgirl (at) archlinux.us>
# Contributor: Jonathan Friedman <jonf@gojon.com>

pkgname=stumpwm-git
# _pkgname=stumpwm
pkgver=95101bd
pkgrel=1
pkgdesc="A tiling, keyboard-driven window manager written in common lisp"
arch=('i686' 'x86_64')
license=('GPL2')
depends=('quicklisp' 'xorg-xdpyinfo')
makedepends=('git' 'texinfo' 'autoconf')
url="http://github.com/stumpwm/stumpwm"
provides=('stumpwm')
optdepends=('emacs: Edit and eval stumpwm code with M-x stumpwm-mode'
            'stumpwm-contrib: A collection of StumpWM modules'
           )

source=('git://github.com/stumpwm/stumpwm.git'
        'stumpwm.desktop'
       )

sha256sums=('SKIP'
            '155e1530975f279d03db9652879d3c61a5cbc398cae3ae74804b03e23e4bb1fb'
           )

pkgver() {
	cd ${srcdir}/${pkgname%%-*}
	git describe --always | sed 's/-/./g'
}

install=stumpwm.install
options=(!strip)  # Thanks to sidereus for pointing this out

_gitroot="https://github.com/stumpwm/stumpwm.git"
# _git_contrib="https://github.com/stumpwm/stumpwm-contrib.git"
_gitname="stumpwm"

build() {

  sbcl --non-interactive \
       --eval "(ql:update-client)" \
       --eval "(ql:update-all-dists)" \
       --eval "(ql:quickload \"clx\")" \
       --eval "(ql:quickload \"cl-ppcre\")"

  msg "Compiling StumpWM..."
  msg "Connecting to ${_gitroot}..."

  if [ -d ${srcdir}/${_gitname} ] ; then
      cd ${srcdir}/${_gitname} && git pull origin master
  else
    git clone $_gitroot
  fi

  msg "GIT checkout done or server timeout"

  msg "Starting make..."
  rm -rf ${srcdir}/${_gitname}-build

  cp -a ${srcdir}/${_gitname} ${srcdir}/${_gitname}-build
  cd ${srcdir}/${_gitname}-build

  ./autogen.sh

  ./configure --prefix=/usr \
              --with-lisp=sbcl \
              --with-sbcl=$(which sbcl)
}

package() {
  msg "Installing StumpWM xsession"
  install -Dm 644 ${srcdir}/stumpwm.desktop ${pkgdir}/usr/share/xsessions/stumpwm.desktop

  # rm -f ${pkgdir}/usr/share/info/dir

  msg "Installing StumpWM"
  cd ${srcdir}/${_gitname}-build
  make destdir=${pkgdir} install

  install -Dm 644 sample-stumpwmrc.lisp ${pkgdir}/etc/stumpwmrc.sample
 }
