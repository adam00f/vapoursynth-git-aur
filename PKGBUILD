# Maintainer: Gustavo Alvarez <sl1pkn07@gmail.com>
# Contributor: jackoneill <cantabile dot desu at gmail dot com>
# Local patch: current VapourSynth git master using pyproject.toml + meson-python

pkgname=vapoursynth-git
pkgver=77.r4735.g33769c54
pkgrel=1
pkgdesc="A video processing framework with simplicity in mind. (GIT version)"
arch=('x86_64')
url='https://www.vapoursynth.com'
license=('LGPL-2.1-or-later' 'custom:WTFPL')
depends=(
  'python'
  'zimg'
)
makedepends=(
  'git'
  'cython'
  'meson'
  'ninja'
  'python-build'
  'python-installer'
  'meson-python'
  'python-wheel'
)
provides=(
  'vapoursynth=77'
  'libvapoursynth.so'
  'libvsscript.so'
)
conflicts=(
  'vapoursynth'
)
source=(
  'git+https://github.com/vapoursynth/vapoursynth.git#branch=master'
  'vapoursynth.xml'
  'wtfpl.txt'
)
sha256sums=(
  'SKIP'
  '8e51579547d20cd7cb9618a47b3ac508423d09d76649bf038d0ab9acb850b068'
  '7637386b5f81e8a719ca336233149005e5fa28b5e6054ea7b67de49355b0ad40'
)
options=('debug')

pkgver() {
  cd vapoursynth

  local _ver _count _hash

  _ver="$(grep -Po "version\s*:\s*'\K[0-9]+" meson.build | head -n1)"
  _count="$(git rev-list --count HEAD)"
  _hash="$(git rev-parse --short HEAD)"

  printf '%s.r%s.g%s' "${_ver:-77}" "${_count}" "${_hash}"
}

prepare() {
  cd vapoursynth
  :
}

build() {
  cd vapoursynth

  python -m build --wheel --no-isolation
}

check() {
  cd vapoursynth

  rm -rf "${srcdir}/test-install"
  python -m installer --destdir="${srcdir}/test-install" dist/*.whl

  local _purelib _platlib
  _purelib="$(python -c 'import sysconfig; print(sysconfig.get_paths()["purelib"])')"
  _platlib="$(python -c 'import sysconfig; print(sysconfig.get_paths()["platlib"])')"

  PYTHONPATH="${srcdir}/test-install${_purelib}:${srcdir}/test-install${_platlib}:${PYTHONPATH}" \
    PATH="${srcdir}/test-install/usr/bin:${PATH}" \
    python - <<'PYCHECK'
import vapoursynth as vs
print(vs.__version__)
print(vs.core.version())
PYCHECK
}

package() {
  depends+=('libzimg.so')

  cd vapoursynth

  python -m installer --destdir="${pkgdir}" dist/*.whl

  install -Dm644 "${srcdir}/vapoursynth.xml" \
    "${pkgdir}/usr/share/mime/packages/vapoursynth.xml"

  install -Dm644 COPYING.LESSER \
    "${pkgdir}/usr/share/licenses/${pkgname}/COPYING.LESSER"

  install -Dm644 "${srcdir}/wtfpl.txt" \
    "${pkgdir}/usr/share/licenses/${pkgname}/wtfpl.txt"
}
