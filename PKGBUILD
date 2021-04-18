#
# Maintaner: Liam Powell
# A fork from AUR's repository:
#   Maintainer: Cameron Katri <katri.cameron@gmail.com>
#   Contributor: Plague-doctor <plague <at>> privacyrequired <<dot>> com >
#   Contributor: me at oguzkaganeren dot com dot tr
#   Contributor: Rowisi < nomail <at> private <dot> com >
#

pkgname=vscodium
pkgver=$(echo $(curl -s https://github.com/VSCodium/vscodium/releases/latest | cut -d \" -f 2) | grep -o '[0-9].*')
pkgrel=2
packager="Liam Powell"
pkgdesc="Visual Studio Code's FOSS alternative."
arch=('x86_64')
url="https://github.com/VSCodium/vscodium"
license=('MIT')
conflicts=(
    'code-git'
    'code-marketplace'
    'code-code-nautilus-git'
    'code-server'
    'code-stable-git'
    'code-transparent'
    'code-wayland'
    'visual-studio-code-bin'
    'visual-studio-code-wayland'
)
depends=(fontconfig libxtst gtk3 python cairo alsa-lib nss gcc-libs libnotify libxss 'glibc>=2.28-4')
optdepends=('gvfs: For move to trash functionality' 'libdbusmenu-glib: For KDE global menu')
provides=('codium')

sha256sums_x86_64=("SKIP")

pkgfile="VSCodium-linux-x64-$(echo $(curl -s https://github.com/VSCodium/vscodium/releases/latest | cut -d \" -f 2) | grep -o '[0-9].*').tar.gz"
source_x86_64=("https://github.com/VSCodium/vscodium/releases/download/$pkgver/$pkgfile")

shopt -s extglob

prepare() {
    sha256_file=$(echo VSCodium-linux-x64-$(echo $(curl -s https://github.com/VSCodium/vscodium/releases/latest | cut -d \" -f 2) | grep -o '[0-9].*').tar.gz.sha256)
    curl -sL https://github.com/VSCodium/vscodium/releases/download/$pkgver/$sha256_file > ../$sha256_file
    sha256sum -c ../$sha256_file
    printf "[Desktop Entry]
Name=VSCodium
Comment=Code Editing. Redefined.
GenericName=Text Editor
Exec=/usr/share/${pkgname}/bin/codium --no-sandbox --unity-launch %F
Icon=vscodium
Type=Application
StartupNotify=true
StartupWMClass=VSCodium
Categories=Utility;Development;IDE;
MimeType=text/plain;inode/directory;
Actions=new-empty-window;
Keywords=vscode;

[Desktop Action new-empty-window]
Name=New Empty Window
Exec=/usr/share/${pkgname}/bin/codium --no-sandbox --new-window %F
Icon=vscodium" > ${srcdir}/${pkgname}.desktop
    if [ ! -d ../pkg ]; then
        mkdir ../pkg
    fi
    chmod 750 ../pkg/
    cp -r -p ${srcdir} ${pkgdir}
}

package() {
    echo "$filename"
    install -d -m755 ${pkgdir}/usr/bin
    install -d -m755 ${pkgdir}/usr/share/{${pkgname},applications,pixmaps}  
    cp -r ${srcdir}/!(${pkgname}.desktop|${pkgfile}) ${pkgdir}/usr/share/${pkgname}
    ln -s /usr/share/${pkgname}/bin/codium ${pkgdir}/usr/bin/codium
    ln -s /usr/share/${pkgname}/bin/codium ${pkgdir}/usr/bin/vscodium
    install -D -m644 ${srcdir}/${pkgname}.desktop ${pkgdir}/usr/share/applications/${pkgname}.desktop
    install -D -m644 ${srcdir}/resources/app/resources/linux/code.png ${pkgdir}/usr/share/pixmaps/vscodium.png
}
