![GITHUB-BADGE](https://github.com/cmangla/AirSane-openwrt/actions/workflows/build.yml/badge.svg)
# AirSane for OpenWRT
This repository contains OpenWRT package for the AirSane project at https://github.com/SimulPiscator/AirSane
## Usage
### Download OpenWRT SDK
The easiest way is to use Docker image\
https://hub.docker.com/r/openwrtorg/sdk

```
docker run --rm -v "$(pwd)"/bin/:/home/build/openwrt/bin -it openwrtorg/sdk:<target>-<subtarget>[-<branch>]
```
If you plan to reuse SDK for other packages or purposes remove ```--rm``` switch


Example for latest (at time of writing) for Xiaomi router 3g (non v2)
```
docker run --rm -v "$(pwd)"/bin/:/home/build/openwrt/bin -it openwrtorg/sdk:ramips-mt7621-21.02.1
```
### Prepare
Add repository
```
echo "src-git airsaned https://github.com/cmangla/AirSane-openwrt.git" >> feeds.conf.default
```

Update feeds
```
./scripts/feeds update base packages airsaned && make defconfig && ./scripts/feeds install airsaned
```
### Compile
```
make package/airsaned/compile V=s -j <CORES_NUM>
```

### Install
1) Copy package to temp folder on your router
```
scp bin/packages/<ARCH>/airsaned/airsaned-<VERSION>.ipk root@<YOUR_ROUTER_IP>:/tmp
```
2) Login via ssh
```
ssh root@<YOUR_ROUTER_IP>
```
3) Install package via opkg
```
opkg install /tmp/airsaned-<VERSION>.ipk
```

### Configure
You can edit configuration here\
```/etc/config/airsaned```

### Use
Run\
```/etc/init.d/airsaned start```

Run at startup\
```/etc/init.d/airsaned enable```
