# Maintainer: Frederik Schwan <frederik dot schwan at linux dot com>

pkgname=gitea
pkgver=1.1.0
pkgrel=3
pkgdesc='Git with a cup of tea, forked from Gogs. Is a Self Hosted Git Service in the Go Programming Language.'
arch=('any')
url='http://gitea.io'
license=('MIT')
depends=('go')
makedepends=('git' 'go-bindata')
optdepends=('sqlite: SQLite support'
            'mariadb: MariaDB support'
            'postgresql: PostgreSQL support'
            'pam: Authentication via PAM support'
            'redis: Redis support'
            'memcached: MemCached support'
            'openssh: GIT over SSH support')
conflicts=('gitea-git' 'gitea-git-dev')
install=gitea.install
backup=('etc/gitea/app.ini')
source=(v${pkgver}::git://github.com/go-gitea/gitea.git
        gitea.service
        app.ini)
sha512sums=('SKIP'
            '692ea79b3195f3222f69b485f8a7905223fa457dc5cb2b480edbac6f480ac4f74075accb04ae0c17b90e98e41f53224e661a85762310d7263921e763cb3fc257'
            'dfeaad31e2a1286d4399627587b827096e5f79411423caecde16f07684b03b18b57023c8e2c4da1045a706098ed887ab4b43d2de197d910f585f7779f1efe500')

prepare() {
  mkdir -p "${srcdir}/src/code.gitea.io"
  ln -s "${srcdir}/v${pkgver}" "${srcdir}/src/code.gitea.io/gitea"
}

build() {
  cd ${srcdir}/src/code.gitea.io/${pkgname}
  GOPATH="${srcdir}" make DESTDIR="${pkgdir}" clean generate build
}

package() {
  install -o git -g git -d -m 750 "${pkgdir}/var/lib/gitea/"
  install -o git -g git -d -m 750 "${pkgdir}/var/lib/gitea/"{repos,tmp,sessions,attachments,data,indexer,conf}
  install -o git -g git -d -m 750 "${pkgdir}/var/log/gitea/"
  install -o root -g git -d -m 775 "${pkgdir}/etc/gitea/"

  install -Dm755 "${srcdir}/src/code.gitea.io/${pkgname}/${pkgname}" "${pkgdir}/usr/bin/${pkgname}"
  install -Dm644 "${srcdir}/gitea.service" "${pkgdir}/usr/lib/systemd/system/${pkgname}.service"
  install -Dm644 "${srcdir}/src/code.gitea.io/${pkgname}/LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
  install -o root -g git -Dm644 "${srcdir}/app.ini" "${pkgdir}/etc/gitea/app.ini"

  cp -r "${srcdir}/src/code.gitea.io/${pkgname}/"{templates,options,public} "${pkgdir}/var/lib/${pkgname}"
  cp -r "${srcdir}/src/code.gitea.io/${pkgname}/options/locale" "${pkgdir}/var/lib/${pkgname}/conf"
}
