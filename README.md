openwrt-gfw自动重拨脚本 openwrt-gfw auto-redial script

6月之后兲朝加大了对vpn的干扰，流量稍大比如google images/maps和言论不受中共歪曲的groups用不了多久便会挂掉。
openwrt pptp拨号会生成2个进程，vpn断线子进程就没了，而openwrt不会自动重拨。但如果杀掉父进程就会自动重拨。
电脑没这问题，可能x86/64性能比risc arm/mips高吧。如果想vpn上大兲朝局域网不被gfw卡死，还得用路由器翻墙。
而openwrt-gfw的内核优化都是针对OpenWrt Attitude Adjustment 12.09，总不能为这个换掉mw4530r吧。
mw4530r性价比很高，现在京东易讯都买不到了，而wr1041n网络不稳定、手上的wr703n配置太低了。
所以我写了个小脚本，每過一秒判断pppd进程剩俩就重拨。一般第一个pptp是pppoe拨号。
#!/bin/sh
while :; do
	if [ `pgrep pppd | wc -l` -lt 3 ]; then
		kill `pgrep pppd | sed -n 2p`	# pppoe, pptp, pptp fork
	fi
	sleep 1
done

1、把这个脚本放/root/下面设置成可执行，然后crontab -e添加*/1 * * * * /root/fkccp；
2、1分钟内会执行fkccp，ps | grep fkccp看到再crontab -e删掉刚才添加这一行；
3、/etc/rc.local里exit 0之前加/root/fkccp，路由器断电/重启就能自动运行；
