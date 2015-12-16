# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor: Massimiliano Torromeo <massimiliano.torromeo@gmail.com>

pkgbase=python-requests
pkgname=('python-requests' 'python2-requests')
pkgver=2.9.0
pkgrel=1
pkgdesc="Python HTTP for Humans"
arch=('any')
url="http://python-requests.org"
license=('Apache')
makedepends=('python-setuptools' 'python2-setuptools' 'python-chardet' 'python2-chardet'
             'python-urllib3' 'python2-urllib3')
checkdepends=('python-pytest-httpbin' 'python2-pytest-httpbin')
source=(http://pypi.python.org/packages/source/r/requests/requests-$pkgver.tar.gz
        certs.patch)
sha256sums=('4881966532b5a36c552244fd909de66d1b8c4a26086f56fd5837cfcde63f8eb8'
            'e35e779d8640f35ea2ea51112f967d927b44d59483af4cd2c0945c84e79bb7c7')

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
    py.test

    cd "$srcdir"/requests-$pkgver-py2
    test -f "$(python2 -m requests.certs)"
    py.test2
}

package_python-requests() {
    depends=('python-urllib3' 'python-chardet')

    cd "$srcdir"/requests-$pkgver
    python setup.py install --skip-build -O1 --root="$pkgdir"
    install -m0644 -D "LICENSE" "$pkgdir"/usr/share/licenses/$pkgname/LICENSE
}

package_python2-requests() {
    depends=('python2-urllib3' 'python2-chardet')
    optdepends=('python2-ndg-httpsclient: HTTPS requests with SNI support'
                'python2-grequests: asynchronous requests with gevent')

    cd "$srcdir"/requests-$pkgver-py2
    python2 setup.py install --skip-build -O1 --root="$pkgdir"
}
