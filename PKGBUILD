# Maintainer: Devin J. Pohly <djpohly+arch@gmail.com>
# Personal dynamic-color DWL package for Amber.
pkgname=dwl
pkgver=0.7
pkgrel=7
pkgdesc="Simple, hackable dynamic tiling Wayland compositor (dwm for Wayland)"
arch=('x86_64')
url="https://codeberg.org/dwl/dwl"
license=('GPL')
depends=('wlroots0.18' 'libxcb' 'xcb-util-wm')
makedepends=('wayland-protocols')
optdepends=('xorg-xwayland: for XWayland support'
            'dwlb: top bar with clickable IPC tags'
            'bemenu: dmenu-like Wayland launcher'
            'wallust: wallpaper-derived colors'
            'swaybg: Wayland wallpaper display'
            'nitrogen: familiar wallpaper picker wrapper'
            'kvantum: Qt theme bridge'
            'qt5ct: Qt 5 color configuration'
            'qt6ct: Qt 6 color configuration'
            'adw-gtk-theme: dark GTK theme base')
source=("https://codeberg.org/dwl/dwl/releases/download/v$pkgver/$pkgname-v$pkgver.tar.gz"
        config.h
        dwl-runtime-colors.patch
        dwl-ipc.patch
        dwl-ipc-unstable-v2.xml
        start-dwl
        dwl-wallust-refresh
        dwl-bemenu-run
        dwl-startup
        dwl-colors
        dwl-wallpaper
        dwl-apply-toolkit-theme
        dwlb-launch
        dwlb-status
        nitrogen)
sha256sums=('16e1412385f5fecaea997f734cb290f2bc412929da5b523c7c5180c6bd9402ea'
            SKIP
            SKIP
            SKIP
            SKIP
            SKIP
            SKIP
            SKIP
            SKIP
            SKIP
            SKIP
            SKIP
            SKIP
            SKIP
            SKIP)

prepare() {
	cd "$srcdir/$pkgname-v$pkgver"
	# Use a custom config.h if the file is not empty
	if [ -s "$srcdir/config.h" ]; then
		cp -f "$srcdir/config.h" config.h
	fi
	patch -Np1 -i "$srcdir/dwl-runtime-colors.patch"
	cp -f "$srcdir/dwl-ipc-unstable-v2.xml" protocols/dwl-ipc-unstable-v2.xml
	patch -Np1 -i "$srcdir/dwl-ipc.patch"
	# Compile with XWayland support for older suckless/X-oriented tools.
	sed -i -e '/-DXWAYLAND\|xcb/s/^#//' config.mk
}

build() {
	cd "$srcdir/$pkgname-v$pkgver"
	make
}

package() {
	cd "$srcdir/$pkgname-v$pkgver"
	make PREFIX="$pkgdir/usr/" install

	install -Dm755 "$srcdir/start-dwl" "$pkgdir/usr/bin/start-dwl"
	install -Dm755 "$srcdir/dwl-wallust-refresh" "$pkgdir/usr/bin/dwl-wallust-refresh"
	install -Dm755 "$srcdir/dwl-bemenu-run" "$pkgdir/usr/bin/dwl-bemenu-run"

	install -Dm755 "$srcdir/dwl-startup" "$pkgdir/usr/lib/dwl-personal/dwl-startup"
	install -Dm755 "$srcdir/dwl-colors" "$pkgdir/usr/lib/dwl-personal/dwl-colors"
	install -Dm755 "$srcdir/dwl-wallpaper" "$pkgdir/usr/lib/dwl-personal/dwl-wallpaper"
	install -Dm755 "$srcdir/dwl-apply-toolkit-theme" "$pkgdir/usr/lib/dwl-personal/dwl-apply-toolkit-theme"
	install -Dm755 "$srcdir/dwlb-launch" "$pkgdir/usr/lib/dwl-personal/dwlb-launch"
	install -Dm755 "$srcdir/dwlb-status" "$pkgdir/usr/lib/dwl-personal/dwlb-status"
	install -Dm755 "$srcdir/nitrogen" "$pkgdir/usr/lib/dwl-personal/nitrogen"
}
