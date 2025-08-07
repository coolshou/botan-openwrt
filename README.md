# botan-openwrt
botan2 for openwrt


1. feed.conf
```
src-git botan https://github.com/coolshou/botan-openwrt.git
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
make package/botan/clean
make package/botan/dirclean
make package/botan/compile V=s 
```