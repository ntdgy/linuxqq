# Maintainer: leaeasy <leaeasy at gmail dot com>
# Maintainer: 7Ji <pugokushin at gmail dot com>

pkgname=wechat-beta-bwrap
pkgver=1.0.0.145
pkgrel=19
pkgdesc="WeChat Testing with bwrap sandbox"
arch=('x86_64' 'aarch64')
url="https://weixin.qq.com"
license=('proprietary')
provides=('wechat-beta')
conflicts=('wechat-beta')
depends=('nss' 'xdg-utils' 'libxss' 'libnotify' 'bubblewrap' 
	'xdg-user-dirs' 'xdg-desktop-portal' 'openssl-1.1' 'lsb-release')
optdepends=(
	'flatpak-xdg-utils: Open files or links with external programs (preferred)'
	'snapd-xdg-open: Open files or links with external programs (fallback)'
)
options=(!strip !debug)
source=(
	wechat.sh
	wechat-beta.desktop
	wechat-beta.png
	license.tar.gz
)

_uosver=2.1.5
_uos_deb_url_common=https://home-store-packages.uniontech.com/appstore/pool/appstore/c/com.tencent.weixin/com.tencent.weixin_
_beta_deb_url_common=https://cdn4.cnxclm.com/uploads/2024/03/05
_beta_deb_id_x86_64=3VDyAc0x
_beta_deb_id_aarch64=NKX87bHT

_uos_deb_stem=wechat-uos_"${_uosver}"
_beta_deb_stem=wechat-beta_"${pkgver}"

source_x86_64=(
	"${_uos_deb_stem}_x86_64.deb::${_uos_deb_url_common}${_uosver}_amd64.deb"
	"${_beta_deb_stem}_x86_64.deb::${_beta_deb_url_common}/${_beta_deb_id_x86_64}_${_beta_deb_stem}_amd64.deb"
)

source_aarch64=(
	"${_uos_deb_stem}_aarch64.deb::${_uos_deb_url_common}${_uosver}_arm64.deb"
	"${_beta_deb_stem}_aarch64.deb::${_beta_deb_url_common}/${_beta_deb_id_aarch64}_wechat-beta_1.0.0.150_arm64.deb" # Upstream provides .150 aarch64 instead of .145
)

noextract=({"${_uos_deb_stem}","${_beta_deb_stem}"}_{x86_64,aarch64}.deb)

sha256sums=(
	'81385310366bb1d62604cc9bd7b8522e4ea9aa92ed3c30dd9343fd4539dd5d43'
	'7692acffebe4ac259cae05d2c92355502fa2cb4ccdbaa27c6cc65f2e1f4678b7'
	'bc13a14c8680daa03c617e71f48419a1b05e2b9d75bb58b15a89d0d191d0fb12'
	'53760079c1a5b58f2fa3d5effe1ed35239590b288841d812229ef4e55b2dbd69'
)

sha256sums_x86_64=(
	'bd537bc3ea0f5cd4cc27f835469c3f0152c8cad31723e80b89e36e75dcb22181'
	'fbb1ada447c2595a4ce568eb79852555a724158836c213c7c2ec366164976ebe'
)

sha256sums_aarch64=(
	'5ef1853d8265b183ea4720f272b046cd07579ffe436b50093f92e0455635a732'
	'0b8a50f194582a0e659075fadc0632feeb303bde80060b210e05cd8427583071'
)

package() {
	echo 'Popupating pkgdir with data from wechat-beta deb file...'
	bsdtar -xOf "${_beta_deb_stem}_${CARCH}.deb" ./data.tar.xz |
		xz -cdT0 |
		bsdtar -xpC "${pkgdir}"

	local _wechat_root="${pkgdir}"/usr/share/wechat-beta
	echo 'Extracting libuosdevicea.so from wechat-uos deb file...'
	bsdtar -xOf "${_uos_deb_stem}_${CARCH}.deb" ./data.tar.xz |
		xz -cdT0 |
		tar -xO ./usr/lib/license/libuosdevicea.so |
		install -Dm644 /dev/stdin \
			"${_wechat_root}"/usr/lib/license/libuosdevicea.so
	install -dm755 "${pkgdir}/usr/lib/license"

	echo 'Fixing licenses...'
	cp -ra license/etc "${_wechat_root}"
	cp -ra license/var "${_wechat_root}"

	echo 'Cleaning unused file...'
	rm -f "${pkgdir}"/usr/share/applications/wechat.desktop

	echo 'Installing desktop files...'
	install -Dm644 wechat-beta.desktop "${pkgdir}"/usr/share/applications/wechat-beta.desktop
	install -Dm755 wechat.sh "${pkgdir}"/usr/bin/wechat-beta
	install -Dm644 wechat-beta.png "${pkgdir}"/usr/share/icons/hicolor/256x256/apps/wechat-beta.png
}
