# Maintainer: Xu Fasheng <fasheng.xu[AT]gmail.com>

### MERGE TO ONE PACKAGE FOR AUR
pkgname='deepin-desktop-environment'
pkgdesc="Linux Deepin Desktop environment"
depends=('deepin-session' 'compiz-core-devel' 'gtk3' 'webkitgtk' 'deepin-webkit' 'gdk-pixbuf2' 'python2' 'dbus-glib' 'sqlite' 'glib2' 'lightdm' 'gstreamer0.10' 'opencv' 'gvfs' 'xdg-user-dirs')

# pkgname=('deepin-desktop-environment'
#          'deepin-desktop-environment-common'
#          'deepin-desktop-environment-dock'
#          'deepin-desktop-environment-launcher'
#          'deepin-desktop-environment-lightdm-greeter'
#          'deepin-desktop-environment-desktop'
#          'deepin-desktop-environment-lock')
# pkgbase="deepin-desktop-environment"
pkgver=git20140313154526
pkgrel=1
arch=('i686' 'x86_64')
url="http://www.linuxdeepin.com/"
license=('GPL2')
# depends=('gtk3' 'webkitgtk' 'deepin-webkit' 'gdk-pixbuf2' 'python2' 'dbus-glib' 'sqlite' 'glib2' 'lightdm' 'gstreamer0.10' 'opencv')
 makedepends=('cmake' 'go' 'coffee-script')
install=deepin-desktop-environment.install

_fileurl=http://packages.linuxdeepin.com/deepin/pool/main/d/deepin-desktop-environment/deepin-desktop-environment_1.0+git20140313154526~7809f4569e~2013.tar.gz
source=("${_fileurl}")
md5sums=('9d6e41e0ac9b064ad5ccdb9d3c57c873')

_filename="$(basename "${_fileurl}")"
_filename="${_filename%.tar.gz}"
_innerdir="${_filename/_/-}"

_install_copyright_and_changelog() {
    mkdir -p "${pkgdir}/usr/share/doc/${pkgname}"
    cp -f debian/copyright "${pkgdir}/usr/share/doc/${pkgname}/"
    gzip -c debian/changelog > "${pkgdir}/usr/share/doc/${pkgname}/changelog.gz"
}

# Usage: _easycp dest files...
_easycp () {
    local dest=$1; shift
    mkdir -p "${dest}"
    cp -R -t "${dest}" "$@"
}

build() {
    cd "${srcdir}/${_innerdir}"
    local _tmpdest="${srcdir}/${_innerdir}/build/_tmpdest"
    mkdir -p "${_tmpdest}"

    (
        mkdir -p build
        cd build
        cmake -DCMAKE_INSTALL_PREFIX="/usr" ..
        make
        make DESTDIR="${_tmpdest}" install
    )

    # fix python version
    find "${_tmpdest}" -iname "*.py" | xargs sed -i 's=\(^#! */usr/bin.*\)python=\1python2='

    gcc -o debian/default-terminal debian/default-terminal.c `pkg-config --libs --cflags glib-2.0 gio-unix-2.0`
}

package_deepin-desktop-environment() {
    pkgdesc='Meta package for Linux Deepin desktop environment'
    depends=('deepin-desktop-environment-dock' 'deepin-desktop-environment-launcher' 'deepin-artwork' 'deepin-desktop-environment-lightdm-greeter' 'deepin-desktop-environment-desktop' 'deepin-desktop-environment-lock')

    cd "${srcdir}/${_innerdir}"
    _install_copyright_and_changelog
}

package_deepin-desktop-environment-common() {
    pkgdesc="Desktop environment resources for Linux Deepin"

    cd "${srcdir}/${_innerdir}"
    local _tmpdest="${srcdir}/${_innerdir}/build/_tmpdest"

    _easycp "${pkgdir}"/usr/share/dde/resources/common/ "${_tmpdest}"/usr/share/dde/resources/common/*
    _easycp "${pkgdir}"/usr/share/dde/data/ "${_tmpdest}"/usr/share/dde/data/*
    _easycp "${pkgdir}"/usr/share/glib-2.0/schemas/ "${_tmpdest}"/usr/share/glib-2.0/schemas/*
    _easycp "${pkgdir}"/usr/share/locale/ "${_tmpdest}"/usr/share/locale/*
    _easycp "${pkgdir}"/etc/sysctl.d/ debian/30-deepin-inotify-limit.conf
    _easycp "${pkgdir}"/usr/bin/ debian/default-terminal

    _install_copyright_and_changelog
}

package_deepin-desktop-environment-dock() {
    pkgdesc="Dock module for  Linuxdeepin test desktop environment"
    depends=('deepin-desktop-environment-common' 'deepin-webkit' 'gvfs' 'xdg-user-dirs')

    cd "${srcdir}/${_innerdir}"
    local _tmpdest="${srcdir}/${_innerdir}/build/_tmpdest"

    _easycp "${pkgdir}"/usr/bin/ "${_tmpdest}"/usr/bin/dock
    _easycp "${pkgdir}"/usr/share/dde/resources/dock/ "${_tmpdest}"/usr/share/dde/resources/dock/*

    mkdir -p "${pkgdir}"/usr/share/applications
    install -m 0644 "${_tmpdest}"/usr/share/applications/deepin-dock.desktop "${pkgdir}"/usr/share/applications/deepin-dock.desktop

    _install_copyright_and_changelog
}

package_deepin-desktop-environment-launcher() {
    pkgdesc="Launcher module for Linux Deepin desktop environment"
    depends=('deepin-desktop-environment-common' 'deepin-webkit' 'gvfs' 'xdg-user-dirs')

    cd "${srcdir}/${_innerdir}"
    local _tmpdest="${srcdir}/${_innerdir}/build/_tmpdest"

    _easycp "${pkgdir}"/usr/bin/ "${_tmpdest}"/usr/bin/launcher
    _easycp "${pkgdir}"/usr/share/dde/resources/launcher/ "${_tmpdest}"/usr/share/dde/resources/launcher/*

    mkdir -p "${pkgdir}"/etc/xdg/autostart
    install -m 0644 "${_tmpdest}"/etc/xdg/autostart/deepin-launcher.desktop "${pkgdir}"/etc/xdg/autostart/

    _install_copyright_and_changelog
}

package_deepin-desktop-environment-desktop() {
    pkgdesc="Desktop module for Linux Deepin desktop environment"
    depends=('deepin-desktop-environment-common' 'deepin-webkit' 'gvfs' 'xdg-user-dirs')

    cd "${srcdir}/${_innerdir}"
    local _tmpdest="${srcdir}/${_innerdir}/build/_tmpdest"

    _easycp "${pkgdir}"/usr/bin/ "${_tmpdest}"/usr/bin/desktop
    _easycp "${pkgdir}"/usr/share/dde/resources/desktop/ "${_tmpdest}"/usr/share/dde/resources/desktop/*

    mkdir -p "${pkgdir}"/usr/share/applications
    install -m 0644 "${_tmpdest}"/usr/share/applications/deepin-desktop.desktop "${pkgdir}"/usr/share/applications/

    _install_copyright_and_changelog
}

package_deepin-desktop-environment-lightdm-greeter() {
    pkgdesc="Lightdm greeter for Linux Deepin test desktop environment"
    depends=('deepin-desktop-environment-common' 'deepin-webkit' 'lightdm' 'opencv')

    cd "${srcdir}/${_innerdir}"
    local _tmpdest="${srcdir}/${_innerdir}/build/_tmpdest"

    _easycp "${pkgdir}"/usr/bin/ "${_tmpdest}"/usr/bin/greeter
    _easycp "${pkgdir}"/usr/share/dde/resources/greeter/ "${_tmpdest}"/usr/share/dde/resources/greeter/*
    _easycp "${pkgdir}"/var/lib/polkit-1/localauthority/50-local.d/ debian/lightdm.pkla

    mkdir -p "${pkgdir}"/usr/share/xgreeters
    install -m 0644 debian/deepin-greeter.desktop "${pkgdir}"/usr/share/xgreeters/

    _install_copyright_and_changelog
}

package_deepin-desktop-environment-lock() {
    pkgdesc="Lock screen module for Linux Deepin desktop environment"
    depends=('deepin-desktop-environment-common' 'deepin-webkit' 'lightdm' 'opencv')

    cd "${srcdir}/${_innerdir}"
    local _tmpdest="${srcdir}/${_innerdir}/build/_tmpdest"

    _easycp "${pkgdir}"/usr/bin "${_tmpdest}"/usr/bin/dlock
    _easycp "${pkgdir}"/usr/bin "${_tmpdest}"/usr/bin/lockservice
    _easycp "${pkgdir}"/usr/bin "${_tmpdest}"/usr/bin/switchtogreeter
    _easycp "${pkgdir}"/usr/share/dbus-1/system-services/ "${_tmpdest}"/usr/share/dbus-1/system-services/*
    _easycp "${pkgdir}"/etc/dbus-1/system.d/ "${_tmpdest}"/etc/dbus-1/system.d/*

    _install_copyright_and_changelog
}

### MERGE TO ONE PACKAGE FOR AUR
package() {
    package_deepin-desktop-environment-common
    package_deepin-desktop-environment-dock
    package_deepin-desktop-environment-launcher
    package_deepin-desktop-environment-lightdm-greeter
    package_deepin-desktop-environment-desktop
    package_deepin-desktop-environment-lock

    # overwrite variables again, or upload to aur may be failed
    pkgname='deepin-desktop-environment'
    pkgdesc="Linux Deepin Desktop environment"
    depends=('deepin-session' 'compiz-core-devel' 'gtk3' 'webkitgtk' 'deepin-webkit' 'gdk-pixbuf2' 'python2' 'dbus-glib' 'sqlite' 'glib2' 'lightdm' 'gstreamer0.10' 'opencv' 'gvfs' 'xdg-user-dirs')
}
