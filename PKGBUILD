# Maintainer: Alexis Polti <ArchSegger at gmail dot com>
# Maintainer: pzl <alsoelp at gmail dot com>

pkgname=jlink-software-and-documentation
pkgver=6.00f
pkgrel=1
epoch=2
pkgdesc="Segger JLink software & documentation pack for Linux"
arch=('i686' 'x86_64')
license=('custom')
groups=('jlink')
depends=('glibc')
source_x86_64=("JLink_Linux_${pkgver/./}_x86_64.tgz::https://www.segger.com/downloads/jlink/JLink_Linux_V${pkgver/./}_x86_64.tgz")
source_i686=("JLink_Linux_${pkgver/./}_i686.tgz::https://www.segger.com/downloads/jlink/JLink_Linux_V${pkgver/./}_i386.tgz")
md5sums_i686=('eb79f86c69a66da48dc3bea88f2e5fdf')
md5sums_x86_64=('e58ea811b8db0b9f06d2bda94b208c56')
install=$pkgname.install
url="https://www.segger.com/jlink-software.html"
conflicts=("j-link-software-and-documentation")
replaces=("j-link-software-and-documentation")
DLAGENTS=("https::/usr/bin/env curl -o %o -d accept_license_agreement=accepted -d confirm=yes ")

prepare() {
    # Change src path name
    if [ ${CARCH} = "i686" ]; then
        mv JLink_Linux_V${pkgver/./}_i386 JLink
    else
        mv JLink_Linux_V${pkgver/./}_x86_64 JLink
    fi
}

package(){
    # Match package placement from their .deb, in /opt
    install -dm755 "${pkgdir}/opt/SEGGER/JLink" \
            "${pkgdir}/usr/share/licenses/${pkgname}" \
            "${pkgdir}/usr/lib/" \
            "${pkgdir}/usr/bin/" \
            "${pkgdir}/etc/udev/rules.d/" \
            "${pkgdir}/usr/share/doc/${pkgname}/"

    cd ${srcdir}/JLink

    # Bulk copy everything
    cp --preserve=mode -r J* Doc Samples README.txt GDBServer x86 lib* "${pkgdir}/opt/SEGGER/JLink"

    # Create links where needed
    ln -s /opt/SEGGER/JLink/Doc/License.txt "${pkgdir}/usr/share/licenses/${pkgname}/"
    install -Dm644 99-jlink.rules "${pkgdir}/etc/udev/rules.d/"
    #install -Dm755 libjlinkarm.so.*.* "${pkgdir}/usr/lib/"
    #install -Dm755 libjlinkarm_x86.so.*.* "${pkgdir}/usr/lib/"
    #ln -s "/usr/lib/libjlinkarm.so.*.*" "${pkgdir}/usr/lib/libjlinkarm.so.${pkgver:0:1}"
    #ln -s "/usr/lib/libjlinkarm_x86.so.*.*" "${pkgdir}/usr/lib/libjlinkarm_x86.so.${pkgver:0:1}"

    for f in J*; do
        ln -s /opt/SEGGER/JLink/"$f" "${pkgdir}/usr/bin"
    done

    for f in Doc/*; do
        ln -s /opt/SEGGER/JLink/"$f" "${pkgdir}/usr/share/doc/${pkgname}"
    done

    # nrfjprog hardcoded libjlinkarm.so* to be in /opt/SEGGER/JLink (will be fixed in later versions)
    #ln -s "/usr/lib/libjlinkarm.so.${pkgver:0:1}" "${pkgdir}/opt/SEGGER/JLink/libjlinkarm.so.${pkgver:0:1}"
}
