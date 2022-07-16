# Maintainer: Alexis Polti <ArchSegger at gmail dot com>
# Maintainer: pzl <alsoelp at gmail dot com>

pkgname=jlink-software-and-documentation
pkgver=6.82e
pkgrel=4
epoch=29
pkgdesc="Segger JLink software & documentation pack for Linux"
arch=('i686' 'x86_64' 'armv7h')
license=('custom')
groups=('jlink')
depends=('glibc' 'libudev0-shim')
source_x86_64=("JLink_Linux_${pkgver/./}_x86_64.tgz::https://www.segger.com/downloads/jlink/JLink_Linux_V${pkgver/./}_x86_64.tgz")
source_i686=("JLink_Linux_${pkgver/./}_i686.tgz::https://www.segger.com/downloads/jlink/JLink_Linux_V${pkgver/./}_i386.tgz")
source_armv7h=("JLink_Linux_${pkgver/./}_arm.tgz::https://www.segger.com/downloads/jlink/JLink_Linux_V${pkgver/./}_arm.tgz")
source=("99-jlink.rules.patch")
md5sums_i686=('0b8458d06e293c1990b417c09b3dd0c1')
md5sums_x86_64=('b51bfc45719c0c44a28e9338b5e6b743')
md5sums_armv7h=('6e2ce839500ad83825e1123a09579f2d')
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
    else if [ ${CARCH} = "x86_64" ]; then
             mv JLink_Linux_V${pkgver/./}_x86_64 JLink
         else mv JLink_Linux_V${pkgver/./}_arm JLink
    	 fi
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
    if [ ${CARCH} = "armv7h" ]; then
        cp --preserve=mode -r J* Devices README.txt GDBServer lib* "${pkgdir}/opt/SEGGER/JLink"
    else cp --preserve=mode -r J* Doc Samples ETC Devices ThirdParty README.txt GDBServer lib* "${pkgdir}/opt/SEGGER/JLink"
    fi
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
