# Maintainer: Alexis Polti <ArchSegger at gmail dot com>
# Maintainer: pzl <alsoelp at gmail dot com>

pkgname=jlink-software-and-documentation
pkgver=6.82
pkgrel=2
epoch=28
pkgdesc="Segger JLink software & documentation pack for Linux"
arch=('i686' 'x86_64')
license=('custom')
groups=('jlink')
depends=('glibc' 'libudev0-shim')
source_x86_64=("JLink_Linux_${pkgver/./}_x86_64.tgz::https://www.segger.com/downloads/jlink/JLink_Linux_V${pkgver/./}_x86_64.tgz")
source_i686=("JLink_Linux_${pkgver/./}_i686.tgz::https://www.segger.com/downloads/jlink/JLink_Linux_V${pkgver/./}_i386.tgz")
source=("99-jlink.rules.patch")
md5sums_i686=('431f93fb8b949e2f0978707ac064353d')
md5sums_x86_64=('a85b116e3c324c973a0a18a8348cc700')
md5sums=('a57d93b791581c1f36e4c672303bb85d')
install=$pkgname.install
url="https://www.segger.com/jlink-software.html"
conflicts=("j-link-software-and-documentation")
replaces=("j-link-software-and-documentation")
DLAGENTS=("https::/usr/bin/env curl -o %o -d accept_license_agreement=accepted -d non_emb_ctr=confirmed")
options=(!strip)

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
            "${pkgdir}/etc/" \
            "${pkgdir}/usr/lib/udev/rules.d/" \
            "${pkgdir}/usr/share/doc/${pkgname}/"

    cd "${srcdir}/JLink"

    # Bulk copy everything
    cp --preserve=mode -r J* Doc Samples ETC Devices ThirdParty README.txt GDBServer lib* "${pkgdir}/opt/SEGGER/JLink"
    if [ ${CARCH} = "x86_64" ]; then
        cp --preserve=mode -r x86 "${pkgdir}/opt/SEGGER/JLink"
    fi

    # Cleanup old copy of /etc/
    rm -f ${pkgdir}/etc/JFlash

    # Create links where needed
    ln -s /opt/SEGGER/JLink/Doc/LicenseIncGUI.txt "${pkgdir}/usr/share/licenses/${pkgname}/"
    sed -i 's/0x//g' 99-jlink.rules
    patch -i "${srcdir}/99-jlink.rules.patch" 99-jlink.rules
    install -Dm644 99-jlink.rules "${pkgdir}/usr/lib/udev/rules.d/"
    rm -f "${pkgdir}/etc/udev/rules.d/99-jlink.rules"

    for f in J*; do
        ln -s /opt/SEGGER/JLink/"$f" "${pkgdir}/usr/bin"
    done
    rm "${pkgdir}/usr/bin/JLinkDevices.xml"

    for f in Doc/*; do
        ln -s /opt/SEGGER/JLink/"$f" "${pkgdir}/usr/share/doc/${pkgname}"
    done
}
