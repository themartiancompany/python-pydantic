# Maintainer: David Runge <dvzrv@archlinux.org>
# Maintainer: Filipe Laíns (FFY00) <lains@archlinux.org>

_name=pydantic
pkgname=python-$_name
pkgver=1.10.8
pkgrel=1
pkgdesc='Data parsing and validation using Python type hints'
arch=(x86_64)
url="https://github.com/pydantic/pydantic"
license=(MIT)
depends=(
  glibc
  python
  python-typing-extensions
)
makedepends=(
  cython
  python-build
  python-installer
  python-importlib-metadata
  python-setuptools
  python-wheel
)
checkdepends=(
  python-hypothesis
  python-pytest
  python-pytest-mock
)
optdepends=(
  'python-dotenv: for .env file support'
  'python-email-validator: for email validation'
)
source=($url/archive/v$pkgver/$_name-v$pkgver.tar.gz)
sha512sums=('3ac41cdf0eb70fb71298131a043966b85387bc953ef2f463ece80728b46251d5d5f66c3f030afc3cdf4527918ae410fcd733a774cbe0c3b7ba9fc806a76378e4')
b2sums=('0b4cc273ce6fad20baa7c8bd87ef32199cb003f52b8e9aa19eda6359ca0e5c30152c7f25d2bb146ec23027011895cf44d9eb051c6ca609fb00e3d7b5f6089e4e')

build() {
  cd $_name-$pkgver
  python -m build --wheel --no-isolation
}

check() {
  local pytest_options=(
    # we don't care about pytest warnings leading to errors
    --pythonwarnings ignore::DeprecationWarning:pkg_resources
  )

  cd $_name-$pkgver
  pytest -vv "${pytest_options[@]}"
}

package() {
  cd $_name-$pkgver
  python -m installer --destdir="$pkgdir" dist/*.whl
  install -vDm 644 LICENSE -t "$pkgdir/usr/share/licenses/$pkgname/"
}
