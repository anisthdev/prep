# Maintainer: asifb
_name=prep
pkgname=totoro-prep
pkgver=0.1.1
pkgrel=1
pkgdesc="Interview preparation tracker: DSA attempts, re-solves, timer, mocks, applications (Python)"
arch=('any')
url="https://github.com/anisthdev/prep"
provides=('prep')
conflicts=('prep')
replaces=('prep')
license=('MIT')
depends=('python')
optdepends=('totoro-quickbar: the bar pill, dashboard and menu')
source=("$_name-$pkgver.tar.gz::$url/archive/v$pkgver.tar.gz")
sha256sums=('0253a44d7b9fca343338975d4306290aa6843e4f6494d0310b59c5199099a4e3')

check() {
    cd "$_name-$pkgver"
    python -m py_compile prep
}

package() {
    cd "$_name-$pkgver"
    install -Dm755 prep "$pkgdir/usr/bin/prep"
    install -Dm644 -t "$pkgdir/usr/share/prep" data/problems.json data/plan.json data/stories.md
    install -Dm644 README.md "$pkgdir/usr/share/doc/$_name/README.md"
    install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$_name/LICENSE"
}
