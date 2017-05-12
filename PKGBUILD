# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor: Massimiliano Torromeo <massimiliano.torromeo@gmail.com>

pkgbase=python-requests
pkgname=('python-requests' 'python2-requests')
pkgver=2.14.2
pkgrel=1
pkgdesc="Python HTTP for Humans"
arch=('any')
url="http://python-requests.org"
license=('Apache')
makedepends=('python-setuptools' 'python2-setuptools' 'python-chardet' 'python2-chardet'
             'python-urllib3' 'python2-urllib3' 'python-pysocks' 'python2-pysocks' 'git')
checkdepends=('python-pytest-httpbin' 'python2-pytest-httpbin' 'python-pytest-mock'
              'python2-pytest-mock')
source=("$pkgbase-$pkgver.tar.gz::https://github.com/kennethreitz/requests/archive/v$pkgver.tar.gz"
        certs.patch)
sha512sums=('a9f2b612bbfc4ce27c9a96bfac052f01329f2adbde7f188ee5507419f73252f282ebe68616df93a8c32680998c85ee7adf683893801df24439de8f0ed02af0a2'
            'b7b2e6aefb8e1ebfa24816bf1a48e45ef27244e23d75e2ac17fc6e8f8fb33846f55c68eca1a03077642dd1fae43b6dfed54e9f5aff382ea4fb4a70287c2fa9ed')

prepare() {
  cd "$srcdir"/requests-$pkgver

  patch -p1 -i "$srcdir"/certs.patch
  sed -r 's#(\W|^)requests/cacert\.pem(\W|$)##' -i MANIFEST.in
  rm -f requests/cacert.pem

  rm -r requests/packages/{urllib3,chardet}
  sed -e '/packages.chardet/d' \
      -e '/packages.urllib3/d' \
      -i setup.py

  cd "$srcdir"
  cp -a requests-$pkgver{,-py2}
  find requests-$pkgver-py2 -name \*.py -exec sed -r 's|^#!(.*)python$|#!\1python2|' -i {} +
}

build() {
  cd "$srcdir"/requests-$pkgver
  python setup.py build

  cd "$srcdir"/requests-$pkgver-py2
  python2 setup.py build
}

check() {
  cd "$srcdir"/requests-$pkgver
  test -f "$(python -m requests.certs)"
  py.test tests

  cd "$srcdir"/requests-$pkgver-py2
  test -f "$(python2 -m requests.certs)"
  py.test2 tests || warning "Tests failed"
}

package_python-requests() {
  depends=('python-urllib3' 'python-chardet')
  optdepends=('python-pysocks: SOCKS proxy support')

  cd "$srcdir"/requests-$pkgver
  python setup.py install --skip-build -O1 --root="$pkgdir"
  install -m0644 -D "LICENSE" "$pkgdir"/usr/share/licenses/$pkgname/LICENSE
}

package_python2-requests() {
  depends=('python2-urllib3' 'python2-chardet')
  optdepends=('python2-ndg-httpsclient: HTTPS requests with SNI support'
              'python2-grequests: asynchronous requests with gevent'
              'python2-pysocks: SOCKS proxy support')

  cd "$srcdir"/requests-$pkgver-py2
  python2 setup.py install --skip-build -O1 --root="$pkgdir"
}
