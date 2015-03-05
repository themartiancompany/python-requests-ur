# Maintainer: Massimiliano Torromeo <massimiliano.torromeo@gmail.com>

pkgbase=python-requests
pkgname=(python-requests python2-requests)
pkgver=2.5.3
pkgrel=1
_libname=requests
pkgdesc="Python HTTP for Humans."
url="http://python-requests.org"
makedepends=('python-setuptools' 'python2-setuptools')
license=('Apache')
arch=('any')
source=(http://pypi.python.org/packages/source/${_libname:0:1}/$_libname/$_libname-$pkgver.tar.gz
        certs.patch)
sha256sums=('55d7f5619daae94ec49ee81ed8c865e5a2a47f0bbf8e06cf94636bee103eaf65'
            'e35e779d8640f35ea2ea51112f967d927b44d59483af4cd2c0945c84e79bb7c7')

prepare() {
    cd "$srcdir"/$_libname-$pkgver
    patch -p1 -i "$srcdir"/certs.patch
    sed -r 's#(\W|^)requests/cacert\.pem(\W|$)##' -i MANIFEST.in
    rm -f requests/cacert.pem
}

build() {
    cd "$srcdir"/$_libname-$pkgver

    rm -rf ../buildpy3; mkdir ../buildpy3
    python setup.py build -b ../buildpy3

    rm -rf ../buildpy2; mkdir ../buildpy2
    python2 setup.py build -b ../buildpy2
    find ../buildpy2 -name \*.py -exec sed -r 's|^#!(.*)python$|#!\1python2|' -i {} +
}

check() {
    cd "$srcdir"/$_libname-$pkgver
    test -f "$(python -m requests.certs)"
}

package_python-requests() {
    depends=(python)

    cd "$srcdir"/$_libname-$pkgver
    rm -rf build; ln -s ../buildpy3 build
    python setup.py install --skip-build -O1 --root="$pkgdir"
    install -m0644 -D "LICENSE" "$pkgdir"/usr/share/licenses/$pkgname/LICENSE
}

package_python2-requests() {
    depends=(python2)
    optdepends=('python2-ndg-httpsclient: HTTPS requests with SNI support'
                'python2-grequests: asynchronous requests with gevent')

    cd "$srcdir"/$_libname-$pkgver
    rm -rf build; ln -s ../buildpy2 build
    python2 setup.py install --skip-build -O1 --root="$pkgdir"
}
