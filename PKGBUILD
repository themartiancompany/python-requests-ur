# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor: Massimiliano Torromeo <massimiliano.torromeo@gmail.com>

pkgbase=python-requests
pkgname=('python-requests' 'python2-requests')
pkgver=2.12.4
pkgrel=1
pkgdesc="Python HTTP for Humans"
arch=('any')
url="http://python-requests.org"
license=('Apache')
makedepends=('python-setuptools' 'python2-setuptools' 'python-chardet' 'python2-chardet'
             'python-urllib3' 'python2-urllib3' 'python-pysocks' 'python2-pysocks' 'git')
checkdepends=('python-pytest-httpbin' 'python2-pytest-httpbin' 'python-pytest-mock'
              'python2-pytest-mock')
source=("git+https://github.com/kennethreitz/requests.git#tag=v$pkgver"
        certs.patch)
sha256sums=('SKIP'
            'e35e779d8640f35ea2ea51112f967d927b44d59483af4cd2c0945c84e79bb7c7')

prepare() {
  cd "$srcdir"/requests

  patch -p1 -i "$srcdir"/certs.patch
  sed -r 's#(\W|^)requests/cacert\.pem(\W|$)##' -i MANIFEST.in
  rm -f requests/cacert.pem

  rm -r requests/packages/{urllib3,chardet}
  sed -e '/packages.chardet/d' \
      -e '/packages.urllib3/d' \
      -i setup.py

  cd "$srcdir"
  cp -a requests{,-py2}
  find requests-py2 -name \*.py -exec sed -r 's|^#!(.*)python$|#!\1python2|' -i {} +
}

build() {
  cd "$srcdir"/requests
  python setup.py build

  cd "$srcdir"/requests-py2
  python2 setup.py build
}

check() {
  cd "$srcdir"/requests
  test -f "$(python -m requests.certs)"
  py.test tests

  cd "$srcdir"/requests-py2
  test -f "$(python2 -m requests.certs)"
  py.test2 tests || warning "Tests failed"
}

package_python-requests() {
  depends=('python-urllib3' 'python-chardet')
  optdepends=('python-pysocks: SOCKS proxy support')

  cd "$srcdir"/requests
  python setup.py install --skip-build -O1 --root="$pkgdir"
  install -m0644 -D "LICENSE" "$pkgdir"/usr/share/licenses/$pkgname/LICENSE
}

package_python2-requests() {
  depends=('python2-urllib3' 'python2-chardet')
  optdepends=('python2-ndg-httpsclient: HTTPS requests with SNI support'
              'python2-grequests: asynchronous requests with gevent'
              'python2-pysocks: SOCKS proxy support')

  cd "$srcdir"/requests-py2
  python2 setup.py install --skip-build -O1 --root="$pkgdir"
}
