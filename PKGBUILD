# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: David Runge <dvzrv@archlinux.org>

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
_pkg=flit-core
_Pkg=flit_core
_name="${_pkg}"
pkgname="${_py}-${_pkg}"
pkgver=3.9.0
pkgrel=1
pkgdesc="A PEP 517 build backend for packages using Flit"
arch=(any)
_http="https://github.com"
_ns="pypa"
url="${_http}/${_ns}/flit/tree/main/flit_core"
license=(BSD)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
  "${_py}-installer"
)

makedepends=(
  "${_py}-build"
  "${_py}-installer"
)
checkdepends=(
  "${_py}-pytest"
  "${_py}-testpath"
)
_pypa="https://files.pythonhosted.org/packages/source"
source=(
  "${_pypa}/${_name::1}/$_name/${_name/-/_}-$pkgver.tar.gz"
)
sha512sums=(
  '1205589930d2c51d6aa6b2533a122a912e63b157e94adba2a0649a58d324fa98a5b84609d9b53e9d236f1cdb6a6984de2cefcf2f11abc2cd83956df21f269ad6'
)
b2sums=(
  '2fb053655a494736f5f9ce2d2c193d5d98622e410c0c0f18c92eb62d32ff98cbe830a1728461ed7e7e087d2fcf5f6a0c912717c2d534be688d688c4714c6865b'
)

build() {
  cd \
    "${_Pkg}-${pkgver}"
  "${_py}" \
    -m \
      build \
    --wheel \
    --skip-dependency-check \
    --no-isolation
}

check() {
  cd \
    "${_Pkg}-${pkgver}"
  "${_py}" \
  pytest \
    -vv
}

package() {
  local \
    site_packages
  site_packages=$( \
    "${_py}" \
      -c \
        "import site; print(site.getsitepackages()[0])")
  cd \
    "${_Pkg}-${pkgver}"
  "${_py}" \
    -m \
      installer \
    --destdir="${pkgdir}" \
    dist/*.whl
  install \
    -vDm644 \
    LICENSE \
    -t \
    "${pkgdir}/usr/share/licenses/${pkgname}/"
  # remove tests
  rm \
    -frv \
    "${pkgdir}/${site_packages}/${_Pkg}/tests/"
  # remove vendored tomli
  rm \
    -frv \
    "${pkgdir}/${site_packages}/${_Pkg}/vendor/"
}
