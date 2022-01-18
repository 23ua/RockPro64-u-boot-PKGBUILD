# U-Boot: RockPro64
# Maintainer: Matyas Mehn <matyas.mehn@tum.de>
# Based on PKGBUILD for uboot-rock64

buildarch=8

pkgname=uboot-rockpro64
pkgver=2022.01
pkgrel=1
pkgdesc="U-Boot for RockPro64"
arch=('aarch64')
url='http://www.denx.de/wiki/U-Boot/WebHome'
license=('GPL')
backup=('boot/boot.txt' 'boot/boot.scr')
makedepends=('bc' 'git' 'arm-none-eabi-gcc' 'arm-none-eabi-newlib')
options=(!buildflags
         !makeflags
         !debug)
install=${pkgname}.install
source=("ftp://ftp.denx.de/pub/u-boot/u-boot-${pkgver/rc/-rc}.tar.bz2"
        "git+https://github.com/ARM-software/arm-trusted-firmware.git"
        'boot.txt'
        'mkscr')
md5sums=('49ba74a214d14c6340c8067bc1ed5fa0'
         'SKIP'
         '0a08740b6f3aef5465eadbbe6ccbbc8a'
         '021623a04afd29ac3f368977140cfbfd')

build() {
  cd u-boot-${pkgver/rc/-rc}
  make distclean
  cd ../

  cd arm-trusted-firmware
  make distclean
  make PLAT=rk3399 bl31

  cd ../
  cp arm-trusted-firmware/build/rk3399/release/bl31/bl31.elf u-boot-${pkgver/rc/-rc}/

  cd u-boot-${pkgver/rc/-rc}

  make rockpro64-rk3399_defconfig
  echo 'CONFIG_IDENT_STRING=" Arch Linux ARM"' >> .config
  make EXTRAVERSION=-${pkgrel}
}

package() {
  cd u-boot-${pkgver/rc/-rc}

  mkdir -p "${pkgdir}/boot"
  mkdir -p "${pkgdir}/usr/share/uboot"

  cp u-boot.itb idbloader.img "${pkgdir}/usr/share/uboot"

  tools/mkimage -A arm -O linux -T script -C none -n "U-Boot boot script" -d ../boot.txt "${pkgdir}/boot/boot.scr"
  cp ../{boot.txt,mkscr} "${pkgdir}"/boot
}
