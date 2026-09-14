# Based on the maple2024 AUR package.

pkgname=maple2026
pkgver=2026.1
pkgrel=1
pkgdesc='A commercial computer algebra system developed by Maplesoft'
arch=('x86_64')
url='https://www.maplesoft.com/products/maple/'
license=('LicenseRef-Maplesoft-EULA')
depends=('alsa-lib'
         'at-spi2-core'
         'cairo'
         'fontconfig'
         'gcc-libs'
         'gdk-pixbuf2'
         'glib2'
         'glibc'
         'gtk3'
         'ld-lsb'
         'libglvnd'
         'libx11'
         'libxext'
         'libxi'
         'libxrender'
         'libxtst'
         'libxxf86vm'
         'pango')
provides=("maple=${pkgver}")
conflicts=('maple18' 'maple2019' 'maple2020' 'maple2021' 'maple2023' 'maple2024')
options=('!strip')
install='maple2026.install'
source=('maple2026.desktop'
        'Maplesoft-x-maple-worksheet.xml'
        'local://Maple2026.1LinuxX64Installer.run')
sha256sums=('e01095dd24dfa692a8ccd7695175ba5f382854cbc3b81defec5f653ed118f0c3'
            '8478a719fd3e393b5bc1a2a92431701a00a15174c1fac5f1798bf36af216f028'
            '4d52414cfc43ca7ef83ef08a84a6c05f09529b1d6049f172d02d0770c2ac8262')

prepare() {
  chmod +x "${srcdir}/Maple2026.1LinuxX64Installer.run"
}

build() {
  printf '%s\n' 'Unpacking the Maple installer (this may take several minutes)'
  "${srcdir}/Maple2026.1LinuxX64Installer.run" \
    --mode unattended \
    --installdir "${srcdir}/maple2026" \
    --desktopshortcut 0 \
    --defaultapp 0 \
    --enableUpdates 0 \
    --checkForUpdatesNow 0
}

package() {
  install -d "${pkgdir}/usr/share/maple2026"
  cp -a "${srcdir}/maple2026/." "${pkgdir}/usr/share/maple2026/"

  rm -rf "${pkgdir}/usr/share/maple2026/uninstall" \
    "${pkgdir}/usr/share/maple2026/update"
  rm -f "${pkgdir}/usr/share/maple2026/license/license.dat"

  install -Dm644 "${srcdir}/maple2026.desktop" \
    "${pkgdir}/usr/share/applications/maple2026.desktop"
  install -Dm644 "${srcdir}/Maplesoft-x-maple-worksheet.xml" \
    "${pkgdir}/usr/share/mime/packages/Maplesoft-x-maple-worksheet.xml"
  install -Dm644 "${srcdir}/maple2026/EULA.html" \
    "${pkgdir}/usr/share/licenses/maple2026/LICENSE.html"

  install -d "${pkgdir}/usr/share/man/man1"
  install -m644 "${srcdir}/maple2026/man/man1/"*.1 \
    "${pkgdir}/usr/share/man/man1/"
  rm -rf "${pkgdir}/usr/share/maple2026/man"

  local _size
  for _size in 16 22 32 48 64 128 256 512; do
    install -Dm644 "${srcdir}/maple2026/data/icons/Maple_${_size}.png" \
      "${pkgdir}/usr/share/icons/hicolor/${_size}x${_size}/apps/maple2026.png"
  done

  install -d "${pkgdir}/usr/bin"
  local _command
  for _command in maple xmaple mint mhelp maple.system.type; do
    ln -s "/usr/share/maple2026/bin/${_command}" \
      "${pkgdir}/usr/bin/${_command}"
  done

  while IFS= read -r _file; do
    sed -i "s|${srcdir}|/usr/share|g" "${_file}"
  done < <(grep -IlR "${srcdir}" "${pkgdir}/usr/share/maple2026" || true)

  find "${pkgdir}/usr/share/maple2026" -type f -name '*.log' -delete
}
