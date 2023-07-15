# Maintainer: David Runge <dvzrv@archlinux.org>
# Maintainer: Filipe Laíns (FFY00) <lains@archlinux.org>

_name=pydantic
pkgname=python-$_name
# WARNING: upstream pins pydantic-core down to the patch-level and using other versions breaks tests! only update in lock-step with python-pydantic-core!
pkgver=2.0.3
pkgrel=1
pkgdesc='Data parsing and validation using Python type hints'
arch=(any)
url="https://github.com/pydantic/pydantic"
license=(MIT)
depends=(
  python
  python-annotated-types
  python-pydantic-core
  python-typing-extensions
)
makedepends=(
  cython
  python-build
  python-installer
  python-importlib-metadata
  python-hatchling
  python-hatch-fancy-pypi-readme
  python-wheel
)
checkdepends=(
  python-ansi2html
  python-devtools
  python-dirty-equals
  python-email-validator
  python-faker
  python-hypothesis
  python-pytest
  python-pytest-examples
  python-pytest-mock
  python-sqlalchemy
)
optdepends=(
  'python-dotenv: for .env file support'
  'python-email-validator: for email validation'
)
source=($url/archive/v$pkgver/$_name-v$pkgver.tar.gz)
sha512sums=('2930cb85b5d4c02bc38d68875e2e5771beb28eb329ce14fcc85d0ed2c27194717e26f22f5fb0a9d548ee2a0ed7172d082a4e9213e55cb971ce8e5f68c0b1b660')
b2sums=('ea04e906c5f16d2a3f962937b6de946b9bb6e3845b11a0d477db047ff502ea0f2ebcea624c53fe5f4590d6164f200c8f8cc1a0d9fe8c6b801a93c5957d305a02')

prepare() {
  # remove benchmark parameters, as they are not recognized by pytest: https://github.com/pydantic/pydantic/issues/6691
  sed -e '/benchmark/d' -i $_name-$pkgver/pyproject.toml
}

build() {
  cd $_name-$pkgver
  python -m build --wheel --no-isolation
}

check() {
  local pytest_options=(
    -vv
    # don't run benchmark tests: https://github.com/pydantic/pydantic/issues/6691
    --ignore tests/benchmarks
    # issues with various doctests: https://github.com/pydantic/pydantic/issues/6656
    --deselect tests/test_docs.py::test_docs_devtools_example
  )
  local site_packages=$(python -c "import site; print(site.getsitepackages()[0])")

  cd $_name-$pkgver
  # install to temporary location, as importlib is used
  python -m installer --destdir=test_dir dist/*.whl
  export PYTHONPATH="$PWD/test_dir/$site_packages:$PYTHONPATH"
  pytest "${pytest_options[@]}"
}

package() {
  cd $_name-$pkgver
  python -m installer --destdir="$pkgdir" dist/*.whl
  install -vDm 644 LICENSE -t "$pkgdir/usr/share/licenses/$pkgname/"
}
