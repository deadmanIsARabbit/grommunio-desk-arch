
pkgname=grommunio-desk
pkgver=1.1.0
pkgrel=1
pkgdesc="Desktop client for grommunio"
arch=('x86_64')
url="https://github.com/grommunio/grommunio-desk"
license=('AGPL-3.0-only')

depends=(
    gtk3
    nss
    alsa-lib
    libsecret
    xdg-utils
)

makedepends=(
    git
    nodejs
    npm
)

source=(
    "git+https://github.com/grommunio/grommunio-desk#tag=v${pkgver}"
)

sha256sums=('SKIP')

options=('!strip')

prepare() {
    cd "$srcdir/$pkgname"

    touch .env

    export npm_config_cache="$srcdir/npm-cache"

    npm ci
}

build() {
    cd "$srcdir/$pkgname"

    export npm_config_cache="$srcdir/npm-cache"

    npm run package
}

package() {
    cd "$srcdir/$pkgname"

    local appdir="out/grommunio Desk-linux-x64"

    install -dm755 \
        "$pkgdir/opt/$pkgname"

    cp -a "$appdir/"* \
        "$pkgdir/opt/$pkgname"

    install -Dm755 /dev/stdin \
        "$pkgdir/usr/bin/$pkgname" <<'EOF'
#!/bin/sh
exec /opt/grommunio-desk/grommunio-desk "$@"
EOF

    install -Dm644 \
        assets/os_icons/app_icon.png \
        "$pkgdir/usr/share/icons/hicolor/512x512/apps/$pkgname.png"

    install -Dm644 \
        LICENSE \
        "$pkgdir/usr/share/licenses/$pkgname/LICENSE.txt"

    install -d "$pkgdir/usr/share/applications"

    cat > "$pkgdir/usr/share/applications/$pkgname.desktop" <<EOF
[Desktop Entry]
Version=1.0
Type=Application
Name=grommunio Desk
Comment=Desktop client for grommunio
Exec=grommunio-desk
Icon=grommunio-desk
Terminal=false
StartupNotify=true
Categories=Office;Network;Email;
MimeType=x-scheme-handler/mailto;
EOF
}
