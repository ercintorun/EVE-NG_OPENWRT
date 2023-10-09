# EVE-NG_OPENWRT
How to create open-wrt linux on EVE-NG

## How to Select WRT Image
https://openwrt.org/docs/guide-user/installation/openwrt_x86 
I've selected 64 (for modern pc) and  squashfs-combined.img.gz to be able to reset configuration etc. 

## Eve-NG Create Linux Image

    cd /opt/unetlab/addons/qemu/linux-openwrt/
    qemu-img create -f qcow2 hda.qcow2 1G
    wget https://archive.openwrt.org/releases/22.03.5/targets/x86/64/openwrt-22.03.5-x86-64-generic-squashfs-combined.img.gz
    gzip -d openwrt-22.03.5-x86-64-generic-squashfs-combined.img.gz 
    dd if=openwrt-22.03.5-x86-64-generic-squashfs-combined.img of=hda.qcow2
    qemu-img resize hda.qcow2 +750M
    rm openwrt-22.03.5-x86-64-generic-squashfs-combined.img 
    /opt/unetlab/wrappers/unl_wrapper -a fixpermissions 

## Open-WRT Configuration 

    root@Openwrt:/# vim /etc/config/network
    config interface 'loopback'
            option ifname 'lo'
            option proto 'static'
            option ipaddr '127.0.0.1'
            option netmask '255.0.0.0'
    
    config globals 'globals'
            option ula_prefix 'fddc:9d6e:885b::/48'

    config interface 'lan'
            option type 'bridge'
            option ifname 'eth0 eth1 eth2 eth3'
            option proto 'static'
            option ipaddr '192.168.1.1'
            option netmask '255.255.255.0'
            option ip6assign '60'
    LAN IP: 192.168.1.1/24
    
    config interface 'wan'
            option ifname 'eth4'
            option proto 'dhcp'
    
    config interface 'wan6'
            option ifname 'eth4'
            option proto 'dhcpv6'
    
    
    config interface 'lan'
            option type 'bridge'
            option ifname 'eth0 eth1 eth2 eth3'
            option proto 'static'
            option ipaddr '10.55.55.25'
            option netmask '255.255.255.224'
            option ip6assign '60'
            option gateway '10.55.55.1'
            option broadcast '255.255.255.255'
            option dns '114.114.114.114 8.8.8.8'
    
    root@Openwrt:/# service network restart
    
## Sources
https://openwrt.org/docs/guide-user/installation/openwrt_x86 
https://gist.github.com/rodrigojusto/684308f6d65ac86a3c845912cee86789 
