# Maintainer: Fredy García <frealgagu at gmail dot com>

native="WhatsApp"
pkgname=whatsapp
pkgver=2.2138.10
pkgrel=1
pkgdesc="WhatsApp desktop built with nativefier (electron)"
arch=("armv7l" "i686" "x86_64")
url="https://web.${pkgname%-nativefier}.com"
license=("custom")
depends=("gtk3" "libxss" "nss")
optdepends=("libindicator-gtk3")
makedepends=("unzip")
source=(
  "${pkgname}.png"
  "${pkgname}.desktop"
  "${pkgname}-inject.js"
)
sha256sums=(
  "3899581abcfed9b40b7208bbbca8bdbfe3ae9655980dbf55f04dec9cb3309f27"
  "8ae9288eb89c12ff81e61fbb492e5f8eafa9f1e2e54752d2f6fea45136fb9b16"
  "25d0587e3c3c9d5262778ece1e24696e51ae7617d3a458bda3d638e1683c9164"
)

build() {
  cd "${srcdir}"
  
  nativefier \
    --name "${native}" \
    --icon "${pkgname}.png" \
    --width "800px" \
    --height "600px" \
    --user-agent "chrome" \
    --inject "${pkgname}-inject.js" \
    --browserwindow-options '{ "webPreferences": { "spellcheck": true } }' \
    --verbose \
    --single-instance \
    --tray \
    "${url}"
}

package() {
  install -dm755 "${pkgdir}/"{opt,usr/{bin,share/{applications,licenses/${pkgname}}}}

  _folder=$(ls "${srcdir}" | grep "[Ww]hats[-]*[Aa]pp-linux-")
  _binary=$(ls "${srcdir}/${_folder}" | grep "[Ww]hats[-]*[Aa]pp")

  cp -rL "${srcdir}/${_folder}" "${pkgdir}/opt/${pkgname}"
  ln -s "/opt/${pkgname}/${_binary}" "${pkgdir}/usr/bin/${pkgname}"
  install -Dm644 "${srcdir}/${pkgname}.desktop" "${pkgdir}/usr/share/applications/${pkgname}.desktop"
  install -Dm644 "${pkgdir}/opt/${pkgname}/LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
  for _size in "192x192" "128x128" "96x96" "64x64" "48x48" "32x32" "24x24" "22x22" "20x20" "16x16" "8x8"
  do
    install -dm755 "${pkgdir}/usr/share/icons/hicolor/${_size}/apps"
    convert "${srcdir}/${pkgname}.png" -strip -resize "${_size}" "${pkgdir}/usr/share/icons/hicolor/${_size}/apps/${pkgname}.png"
  done
}
