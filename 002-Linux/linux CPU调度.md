### linux CPU调度
#### 安装cpupower
```
apt install linux-cpupower
```
#### 相关命令
```
#可用的模式
cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_available_governors

#设置到性能模式
cpupower -c all frequency-set -g performance

#查看当前所处模式
cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor

#设置到省电模式
cpupower -c all frequency-set -g powersave
```