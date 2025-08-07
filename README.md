# botan-openwrt
botan2 for openwrt


1. feed.conf
```
src-git botan2 https://github.com/coolshou/botan-openwrt.git
```

2. build
```
./scripts/feeds update -a
./scripts/feeds install -a
```

3. config : Libraries --> botan --> botan2
```
make menuconfig
```
4. make package
```
make package/botan2/download
make package/botan2/clean
make package/botan2/dirclean
make package/botan2/compile V=s 
```