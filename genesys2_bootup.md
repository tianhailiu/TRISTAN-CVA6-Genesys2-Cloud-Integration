setenv bootcmd 'mmc info; mmc read 0x90000000 0x100000 0x61A8; setenv fdt_high 0xffffffffffffffff; bootm 0x90000000 - ${fdtcontroladdr}'


ip addr
ip link set eth0 up
ip link set eth0 mtu 1200
udhcpc -i eth0
ip addr

echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
echo "PasswordAuthentication yes" >> /etc/ssh/sshd_config

ps | grep sshd
killall sshd
/usr/sbin/sshd

ip link set lo up
ip addr add 127.0.0.1/8 dev lo 2>/dev/null || true
ip -6 addr add ::1/128 dev lo 2>/dev/null || true

mosquitto -d

passwd root



scp CloudConnector-riscv root@192.168.68.105:/

39h1odnwr0l65rsnnqqn

./CloudConnector-pc --token=39h1odnwr0l65rsnnqqn -s tcp://demo-jamaicaedg.aicas.com:1883  -n 40 -f 1

./CloudConnector-pc &
PID=$!
wait $PID
grep -E 'VmPeak|VmHWM' /proc/$PID/status



/home/liu/Distributions/JamaicaVM-8.7.1-14101-linux-x86_64-linux-riscv64/bin/jamaicavmp -jar cloud-connector-1.0.0-SNAPSHOT-jar-with-dependencies.jar --token=tccdd-token -s tcp://test.mosquitto.org:1883  -n 50 -f 8

/home/liu/Distributions/JamaicaVM-8.7.1-14101-linux-x86_64-linux-riscv64/bin/jamaicavmp -jar cloud-connector-1.0.0-SNAPSHOT-jar-with-dependencies.jar --token=tccdd-token -s tcp://test.mosquitto.org:1883  -n 50 -f 8

/home/liu/Distributions/JamaicaVM-8.7.1-14101-linux-x86_64-linux-riscv64/bin/profileanalyzer -useProfile cloud-connector-1.0.0-SNAPSHOT-jar-with-dependencies.prof

/home/liu/Distributions/JamaicaVM-8.7.1-14101-linux-x86_64-linux-riscv64/bin/jamaicabuilder -target linux-riscv64 -@profiled.opt -jar cloud-connector-1.0.0-SNAPSHOT-jar-with-dependencies.jar -destination CloudConnector-riscv

/home/liu/Distributions/JamaicaVM-8.7.1-14101-linux-x86_64-linux-riscv64/bin/jamaicabuilder -@profiled.opt -jar cloud-connector-1.0.0-SNAPSHOT-jar-with-dependencies.jar -destination CloudConnector-pc
