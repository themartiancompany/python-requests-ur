# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2024, 2025, 2026  Pellegrino Prevete
#
#    All rights reserved
#    ----------------------------------------------------------------------
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Affero General Public License for more details.
#
#    You should have received a copy of the GNU Affero General Public License
#    along with this program.  If not, see <https://www.gnu.org/licenses/>.

# Maintainers:
#   Truocolo
#     <truocolo@aol.com>
#     <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
#   Pellegrino Prevete (dvorak)
#     <pellegrinoprevete@gmail.com>
#     <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
#   Levente Polyak
#     <anthraxx[at]archlinux[dot]org>
#   Daniel M. Capella <polyzen@archlinux.org>
# Contributors:
#   Felix Yan
#     <felixonmars@archlinux.org>
#   Massimiliano Torromeo
#     <massimiliano.torromeo@gmail.com>

_os="$(
  uname \
    -o)"
_evmfs_available="$(
  command \
    -v \
    "evmfs" || \
    true)"
if [[ ! -v "_evmfs" ]]; then
  if [[ "${_evmfs_available}" != "" ]]; then
    _evmfs="true"
  elif [[ "${_evmfs_available}" == "" ]]; then
    _evmfs="false"
  fi
fi
if [[ ! -v "_git" ]]; then
  _git="false"
fi
if [[ ! -v "_git_service" ]]; then
  _git_service="github"
fi
if [[ ! -v "_tag_name" ]]; then
  _tag_name="pkgver"
  _tag_name="commit"
fi
if [[ ! -v "_ns" ]]; then
  if [[ "${_tag_name}" == "pkgver" ]]; then
    _ns="psf"
  elif [[ "${_tag_name}" == "commit" ]]; then
    _ns="themartiancompany"
  fi
fi
if [[ ! -v "_archive_format" ]]; then
  if [[ "${_git}" == "true" ]]; then
    if [[ "${_evmfs}" == "true" ]]; then
      _archive_format="bundle"
    elif [[ "${_evmfs}" == "false" ]]; then
      _archive_format="git"
    fi
  elif [[ "${_git}" == "false" ]]; then
    _archive_format="tar.gz"
    if [[ "${_tag_name}" == "commit" ]]; then
      if [[ "${_git_service}" == "github" ]]; then
        _archive_format="zip"
      fi
    fi
  fi
fi
if [[ ! -v "_docs" ]]; then
  _docs="true"
  if [[ "${_os}" == "Android" ]]; then
    _docs="false"
  fi
fi
_py="python"
_pyver="$(
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$((
  ${_pyminver} + 1))"
_pkg=requests
pkgbase="${_py}-${_pkg}"
pkgname=(
  "${pkgbase}"
)
_commit="0e322af87745eff34caffe4df68456ebc20d9068"
pkgver=2.32.3
pkgrel=3
pkgdesc='Python HTTP for Humans'
arch=(
  'any'
)
url="https://${_pkg}.readthedocs.io"
license=(
  'Apache-2.0'
)
depends=(
  "ca-certificates"
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
  "${_py}-charset-normalizer"
  "${_py}-idna"
  "${_py}-urllib3"
)
if [[ "${_os}" == "Android" ]]; then
  depends+=(
    # "${_py}-certifi"
  )
fi
makedepends=(
  "${_py}-build"
  "${_py}-installer"
  "${_py}-setuptools"
  "${_py}-wheel"
)
if [[ "${_git}" == "true" ]]; then
  makedepends=(
    "git"
  )
fi
if [[ "${_evmfs}" == "true" ]]; then
  makedepends=(
    "evmfs"
  )
fi
checkdepends=(
  "${_py}-pysocks"
  "${_py}-pytest-httpbin"
  "${_py}-trustme"
)
_python_chardet_optdepends=(
  "${_py}-chardet:"
    "alternative character encoding library"
)
_python_pysocks_optdepends=(
  "${_py}-pysocks:"
    "SOCKS proxy support"
)
optdepends=(
  "${_python_chardet_optdepends[*]}"
  "${_python_pysocks_optdepends[*]}"
)
_http="https://${_git_service}.com"
_url="${_http}/${_ns}/${_pkg}"
if [[ "${_tag_name}" == "pkgver" ]]; then
  _tag="${pkgver}"
elif [[ "${_tag_name}" == "commit" ]]; then
  _tag="${_commit}"
fi
_tarname="${pkgbase}-${_tag}"
_tarfile="${_tarname}.${_archive_format}"
_bundle_sum="8ce03d6392d3aab1c1d9a1aa20e4795ea341ded775a70e8e2b941bdbce7feaa1"
_bundle_sig_sum="645f11d7f4a864c22fa840c9e4a6a5c769ff0a804935e01e4c23ec5dd0d0e12b"
_release_sum='b24a47bc37ffb14fee2d9525b4aa0b86eeb2aab24755fd6e74707c4e4d0b807a'
_release_sig_sum="9031637a3fdbd932db3cb1a976086ff890704c08429d26cd64f606459682a754"
_uri="${url}/archive/v${pkgver}.tar.gz"
_gitlab_sum="728ece7c8aea5504dd921850e40821768169a846dd5a825087ed5ade2636d91b"
_gitlab_sig_sum="ab3de9fafca46637417c2aeab9e4aac41643c1ee61ec5ed958e4d23d2c91ac15"
_github_sum="706ac73e0ff00779543dc4dceaf11127cc1d927c5bc551d74729107a70d9a311"
_github_sig_sum="7fd5089db4a530bf3a3e4464e3c34f4471aee46e1988ace32f74d92d1fafcbb5"
if [[ "${_git_service}" == "gitlab" ]]; then
  if [[ "" == "pkgver" ]]; then
    _sum="${_release_sum}"
    _sig_sum="${_release_sig_sum}"
  fi
  _sum="${_gitlab_sum}"
  _sig_sum="${_gitlab_sig_sum}"
  # Dvorak
  _evmfs_ns="0x87003Bd6C074C713783df04f36517451fF34CBEf"
elif [[ "${_git_service}" == "github" ]]; then
  _sum="${_github_sum}"
  _sig_sum="${_github_sig_sum}"
  # Truocolo
  _evmfs_ns="0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b"
  # Dvorak
  _evmfs_ns="0x87003Bd6C074C713783df04f36517451fF34CBEf"
fi
_evmfs_network="100"
_evmfs_address="0x69470b18f8b8b5f92b48f6199dcb147b4be96571"
_evmfs_dir="evmfs://${_evmfs_network}/${_evmfs_address}/${_evmfs_ns}"
_evmfs_uri="${_evmfs_dir}/${_sum}"
_evmfs_src="${_tarfile}::${_evmfs_uri}"
_sig_uri="${_evmfs_dir}/${_sig_sum}"
_sig_src="${_tarfile}.sig::${_sig_uri}"
source=(
  'certs.patch'
  'certs.termux.patch'
)
sha256sums=(
  "58fbd4a5aa300ce286a336571d495a0846ede2701d8725f6da13a925046b467a"
  "1d7e825074f8ea9d33a11ce546f778c31360571226f235baff3cd19bbf47b651"
)
if [[ "${_evmfs}" == "true" ]]; then
  if [[ "${_git}" == "false" ]]; then
    source+=(
      "${_sig_src}"
    )
    sha256sums+=(
      "${_sig_sum}"
    )
  elif [[ "${_git}" == "true" ]]; then
    _evmfs_uri="${_evmfs_dir}/${_bundle_sum}"
  fi
  _src="${_tarfile}::${_evmfs_uri}"
elif [[ "${_evmfs}" == "false" ]]; then
  if [[ "${_git}" == true ]]; then
    _src="${_tarname}::git+${_url}#${_tag_name}=${_tag}?signed"
    _sum="SKIP"
  elif [[ "${_git}" == false ]]; then
    _uri=""
    if [[ "${_git_service}" == "github" ]]; then
      if [[ "${_tag_name}" == "commit" ]]; then
        _uri="${_url}/archive/${_commit}.${_archive_format}"
        _sum="${_github_sum}"
      elif [[ "${_tag_name}" == "pkgver" ]]; then
        _uri="${_url}/archive/v${pkgver}.${_archive_format}"
        _sum="${_release_sum}"
      fi
    elif [[ "${_git_service}" == "gitlab" ]]; then
      if [[ "${_tag_name}" == "commit" ]]; then
        _uri="${_url}/-/archive/${_tag}/${_tag}.${_archive_format}"
      fi
    fi
    _src="${_tarfile}::${_uri}"
  fi
fi
source+=(
  "${_src}"
)
sha256sums+=(
  "${_sum}"
)
if [[ "${_evmfs}" == "false" ]]; then
  if [[ "${_git}" == "true" ]]; then
    validpgpkeys=(
      # Nathanael Prewitt <nate.prewitt@gmail.com>
      "87227E29AD9CFF5CFAC3EA6A44D3FF97B80DC864"
    )
  elif [[ "${_git}" == "false" ]]; then
    true
  fi
elif [[ "${_evmfs}" == "true" ]]; then
  validpgpkeys=(
    # Truocolo
    #   <truocolo@aol.com>
    '97E989E6CF1D2C7F7A41FF9F95684DBE23D6A3E9'
    'DD6732B02E6C88E9E27E2E0D5FC6652B9D9A6C01'
    # Pellegrino Prevete (dvorak)
    #   <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
    '12D8E3D7888F741E89F86EE0FEC8567A644F1D16'
  )
fi

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
    "../${_patch}"
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
    "tests/test_requests.py::TestRequests::test_unicode_header_name"
}

package() {
  cd \
    "${_pkg}"
  "${_py}" \
    -m \
      "installer" \
    --destdir="${pkgdir}" \
    "dist/"*".whl"
}

# vim: ts=2 sw=2 et:
