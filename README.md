# OpenBMC fork over OpenWRT
## Building with OpenWRT @ 25 of June
```sh
git clone https://github.com/openwrt/openwrt.git; cd openwrt
git checkout 558b38f4b8e615fb5262c94dad7db2071e394ac1
cp feeds.conf{.default,}
cat <<EOF>>feeds.conf
src-git openbmc_target https://github.com/ya-mouse/openbmc-target.git
src-git openbmc_packages https://github.com/ya-mouse/openbmc-packages.git
EOF
./scripts/feeds update
patch -p1 < feeds/openbmc_target/openwrt-558b38f.patch
./scripts/feeds install -p openbmc_target aspeed
./scripts/feeds install -a -p openbmc_target
# bogus collecting info script, remove temp files and re-run
rm -rf tmp; ./scripts/feeds install -a -p openbmc_target
./scripts/feeds install -a -p packages
./scripts/feeds install -a -p luci
./scripts/feeds install -a -p openbmc_packages
cp feeds/openbmc_target/aspeed/configs/config_ast2150 .config
make oldconfig
time make
# ...and make some coffee
```
