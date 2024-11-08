# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Levente Polyak <anthraxx[at]archlinux[dot]org>
# Maintainer: Daniel M. Capella <polyzen@archlinux.org>
# Contributor: Felix Yan <felixonmars@archlinux.org>
# Contributor: Massimiliano Torromeo <massimiliano.torromeo@gmail.com>

_os="$( \
  uname \
    -o)"
_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
_pkg=requests
pkgname="${_py}-${_pkg}"
pkgver=2.32.3
pkgrel=1
pkgdesc='Python HTTP for Humans'
arch=(
  any
)
url="https://${_pkg}.readthedocs.io"
license=(
  Apache-2.0
)
depends=(
  "ca-certificates"
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
  "${_py}-charset-normalizer"
  "${_py}-idna"
  "${_py}-urllib3"
)
makedepends=(
  git
  "${_py}-build"
  "${_py}-installer"
  "${_py}-setuptools"
  "${_py}-wheel"
)
checkdepends=(
  "${_py}-pysocks"
  "${_py}-pytest-httpbin"
  "${_py}-trustme"
)
optdepends=(
  "${_py}-chardet: alternative character encoding library"
  "${_py}-pysocks: SOCKS proxy support"
)
_http="https://github.com"
_ns="psf"
_url="${_http}/${_ns}/${_pkg}"
source=(
  "git+${_url}.git#tag=v${pkgver}"
)
b2sums=(
  '0029d98ac95d27ba56401056d2b380b76e76fc582e596a6c8ba9e4f6197f919876351e88c047098934e31f2e53e88c8f1a31be389d67236233ec971cb510fb8d'
)
if [[ "${_os}" == "Android" ]]; then
  depends+=(
    # "${_py}-certifi"
  )
  source+=(
    certs.termux.patch
  )
  b2sums+=(
    '434b0e8079037182b63b41471d5a616ca0bd1424dbfdd129707c008b427f4aae657c09adf9677f8d8e148c837030cfa283445b510b921346f8cbffc2a243d68a'
    # 'SKIP'
  )
elif [[ "${_os}" == "GNU/Linux" ]]; then
  source+=(
    certs.patch
  )
  b2sums+=(
    '30fc6f283f2416318a1011bffab1ee23b0551188704eeacac77b28f5709f42fc33755a14a2eeb3ba2dccb2904a97a87021cff1423fe9149c78f2b9560998308d'
  )
fi

validpgpkeys=(
  # Nathanael Prewitt <nate.prewitt@gmail.com>
  87227E29AD9CFF5CFAC3EA6A44D3FF97B80DC864
)

prepare() {
  local \
    _patch
  cd \
    "${_pkg}"
    sed \
      -i \
      '/certifi/d' \
      setup.py
  if [[ "${_os}" == "Android" ]]; then
    # no idea which of the certificates
    # unbundled by update-ca-certificates
    # is the one to select so we'll use certifi
   _patch="certs.termux.patch"
  elif [[ "${_os}" == "GNU/Linux" ]]; then
    _patch="certs.patch"
  fi
  patch \
    -p1 \
    -i \
    ../"${_patch}"
}

build() {
  cd "${_pkg}"
  "${_py}" \
    -m \
      build \
    --wheel \
    --no-isolation
}

check() {
  cd \
    "${_pkg}"
  # test_unicode_header_name hangs
  PYTHONPATH="$PWD/src" \
  pytest \
  -v \
  tests \
  --deselect \
    tests/test_requests.py::TestRequests::test_unicode_header_name
}

package() {
  cd \
    "${_pkg}"
  "${_py}" \
    -m installer \
    --destdir="${pkgdir}" \
    dist/*.whl
}

# vim: ts=2 sw=2 et:
