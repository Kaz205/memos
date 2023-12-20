# Maintainer: Lindsay Zhou <i@lin.moe>

pkgname="memos"
pkgver=0.18.1
pkgrel=1
pkgdesc="A privacy-first, lightweight note-taking service. Easily capture and share your great thoughts."
url="https://github.com/usememos/memos"
arch=("any")
license=('MIT')
makedepends=("go" "git" "nodejs")
provides=("$pkgname")
backup=('etc/memos.conf')
source=(
  "git+https://github.com/Kaz205/memos.git#branch=main"
  "systemd.service"
  "sysusers.conf"
  "memos.conf"
  'tmpfiles.conf'
)
sha512sums=('SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP')

build(){
    # build frontend
    cd "$srcdir/$pkgname/web"
    mkdir -p "$srcdir/bin"
    corepack enable --install-directory "$srcdir/bin"
    
    export PATH="$PATH:$srcdir/bin"
    pnpm install --store-dir=$srcdir/pnpm-store --frozen-lockfile
    pnpm type-gen
    pnpm build
    cp -r "dist" "$srcdir/$pkgname/server/frontend/"

    cd "$srcdir/$pkgname"
    CGO_ENABLED=0 go build -o memos ./bin/memos/main.go
}

check(){
    cd "$srcdir/$pkgname"
    go test ./...
}

package () {
  install -vDm644 systemd.service "$pkgdir/etc/systemd/system/${pkgname}.service"
  install -vDm644 sysusers.conf "$pkgdir/usr/lib/sysusers.d/${pkgname}.conf"
  install -vDm644 tmpfiles.conf "$pkgdir/usr/lib/tmpfiles.d/${pkgname}.conf"
  install -vDm644 memos.conf "$pkgdir/etc/memos.conf"

  cd "$pkgname"
  install -Dm755 "memos" "$pkgdir/usr/bin/memos"
  install -Dm0644 LICENSE "$pkgdir/usr/share/licenses/${pkgname}/LICENSE"
}
