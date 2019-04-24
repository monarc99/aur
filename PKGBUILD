# Contributor: Caleb Maclennan <caleb@alerque.com>
# Contributor: Jacob Mischka <jacob@mischka.me>
# Contributor: Manuel Mazzuola <origin.of@gmail.com>
# Contributor: Simón Oroño <simonorono@protonmail.com>
# Contributor: now-im <now im 627 @ gmail . com>
# Contributor: Giusy Digital <kurmikon at libero dot it>
# Mantainer: Andrés Rodríguez <hello@andres.codes>
# https://aur.archlinux.org/packages/brave-bin/

pkgname=brave-bin
pkgver=0.63.48
pkgrel=1
pkgdesc="Web browser that blocks ads and trackers by default (binary release)."
arch=("x86_64")
url="https://brave.com/download"
license=("custom")
depends=("gtk3" "nss" "alsa-lib" "libxss" "ttf-font")
optdepends=("cups: Printer support"
            "pepper-flash: Adobe Flash support"
            "libgnome-keyring: Enable GNOME keyring support")
provides=("${pkgname-bin}" "brave-browser")
conflicts=("${pkgname-bin}")
source=("$pkgname-$pkgver.zip::https://github.com/brave/brave-browser/releases/download/v${pkgver}/brave-v${pkgver}-linux-x64.zip"
        "$pkgname-$pkgver.deb::https://github.com/brave/brave-browser/releases/download/v${pkgver}/brave-browser_${pkgver}_amd64.deb"
        "MPL2::https://raw.githubusercontent.com/brave/brave-browser/master/LICENSE"
        "$pkgname.sh"
        "$pkgname.desktop"
        "logo.png")
options=(!strip)
sha512sums=("0d3f615a69ee1831ddb6ef8a2bb9f879ea8424614ce7d6a876fccb44508b1041881aa9023f0908e7929db0af0b5f04855398d5a756f68bfb341e097adfdecf86"
            "fa5e389147508ce9217ee924757a92aa78616c26f0eef4fdbee66edc43c1537de315143d77a56e24165e30f1071861faad9556bd3595b8fd8a335d3c92c53d45"
            "b8823586fead21247c8208bd842fb5cd32d4cb3ca2a02339ce2baf2c9cb938dfcb8eb7b24c95225ae625cd0ee59fbbd8293393f3ed1a4b45d13ba3f9f62a791f"
            "20b010e199127fa185da2e78eb97724a1b4d6d279c79b87bb0901ceb832d19ea755485c9039d06d92e6ffd686683990cd1939dc78f37859a798f4a8ba40e05b5"
            "c21aecaafec43bc1ce1ea3439667efb4c7ea5e54bfa87346a9ae9650de1e90c80174b1610a9216f936f693593816c9585c6be1875b3bd318d067079c06251e92"
            "d7bef52e336bd908d24bf3a084a1fc480831d27a3c80af4c31872465b6a0ce39bdf298e620ae9865526c974465807559cc75610b835e60b4358f65a8a8ff159e")
noextract=("$pkgname-$pkgver.zip")

prepare() {
  mkdir -p brave
  cat $pkgname-$pkgver.zip | bsdtar -xf- -C brave
  chmod +x brave/brave
}

_bsdtardir="brave"
_debpack="data.tar.xz"

package() {
    install -d -m0755 "$pkgdir/usr/lib"
    cp -a --reflink=auto $_bsdtardir "$pkgdir/usr/lib/$pkgname"

    install -Dm0755 "$pkgname.sh" "$pkgdir/usr/bin/brave"
    install -Dm0644 -t "$pkgdir/usr/share/applications" "$pkgname.desktop"
    install -Dm0644 "logo.png" "$pkgdir/usr/share/pixmaps/brave.png"
    install -Dm0664 -t "$pkgdir/usr/share/licenses/$pkgname" "MPL2"
    mv "$pkgdir/usr/lib/$pkgname/"{LICENSE,LICENSES.chromium.html} "$pkgdir/usr/share/licenses/$pkgname"

    ln -s /usr/lib/PepperFlash "$pkgdir/usr/lib/pepperflashplugin-nonfree"

    # Copy resources directory from deb package
    ar x $pkgname-$pkgver.deb $_debpack
    mkdir -p deb
    tar -C ./deb -xvf $_debpack ./opt/brave.com/brave/resources/
    cp -a --reflink=auto deb/opt/brave.com/brave/resources/ "$pkgdir/usr/lib/$pkgname"
}
