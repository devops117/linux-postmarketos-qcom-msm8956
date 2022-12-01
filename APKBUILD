_flavor="postmarketos-qcom-msm8956"
pkgname=linux-$_flavor
pkgver=5.10.25
pkgrel=1
pkgdesc="Mainline kernel fork for Qualcomm MSM8956 devices"
arch="aarch64"
_carch="arm64"
url="https://github.com/Kiciuk/linux"
license="GPL-2.0-only"
options="!strip !check !tracedeps
	pmb:cross-native
	pmb:kconfigcheck-nftables
"
makedepends="
	bison
	findutils
	flex
	openssl-dev
	perl
	postmarketos-installkernel
"

# Source
_repository="linux"
_commit="256be394d37897d63911c95bd902548cd97089ce"
source="
	$pkgname-$_commit.tar.gz::https://github.com/Kiciuk/$_repository/archive/$_commit.tar.gz
	config-$_flavor.$arch
"
builddir="$srcdir/linux-$_commit"

prepare() {
	default_prepare
	cp "$srcdir/config-$_flavor.$arch" .config
}

build() {
	unset LDFLAGS
	make ARCH="$_carch" CC="${CC:-gcc}" \
		KBUILD_BUILD_VERSION="$((pkgrel + 1))-$_flavor"
}

package() {
	mkdir -p "$pkgdir"/boot
	make zinstall modules_install dtbs_install \
		ARCH="$_carch" \
		INSTALL_MOD_STRIP=1 \
		INSTALL_PATH="$pkgdir"/boot \
		INSTALL_MOD_PATH="$pkgdir" \
		INSTALL_DTBS_PATH="$pkgdir/usr/share/dtb"

	install -D "$builddir"/include/config/kernel.release \
		"$pkgdir/usr/share/kernel/$_flavor/kernel.release"

