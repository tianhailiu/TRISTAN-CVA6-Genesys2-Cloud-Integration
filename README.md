# TRISTAN CVA6 Genesys2 Cloud Integration Demo

This repository contains the artifacts and instructions to reproduce the TRISTAN demonstration using the CVA6 RISC-V platform on the Digilent Genesys2 FPGA board.

The demo showcases a CloudConnector application running on CVA6 (RISC-V Linux) communicating with an MQTT broker.

---

## 1. Repository Overview

This demo depends on the following repositories:

- CVA6 hardware + bitstream:
  https://github.com/tianhailiu/cva6

- CVA6 SDK (bootloader, Linux, rootfs, etc.):
  https://github.com/tianhailiu/cva6-sdk

- This repository (application + instructions):
  https://github.com/tianhailiu/TRISTAN-CVA6-Genesys2-Cloud-Integration

Artifacts provided here:
- `CloudConnector-riscv` → runs on Genesys2 (RISC-V)
- `CloudConnector-pc` → runs on x86_64 Linux (for comparison/testing)

---

## 2. Hardware Requirements

- Digilent Genesys2 FPGA board
- SD card
- USB drive (for bitstream)
- Ethernet connection
- UART connection to host PC

---

## 3. Build Instructions

### 3.1 Build CVA6 Bitstream

Follow the instructions in:
https://github.com/tianhailiu/cva6

Use the 64-bit configuration and ensure you are using this fork (not upstream).

Output:
- FPGA bitstream

---

### 3.2 Build Software Stack (Linux Image)

Follow:
https://github.com/tianhailiu/cva6-sdk

Use:
- 64-bit configuration
- Genesys2 target

Output:
- Linux kernel + rootfs image for SD card

---

## 4. Deployment

### 4.1 Flash Hardware

- Write bitstream to USB
- Write Linux image to SD card

Insert both into the Genesys2 board.

---

### 4.2 Connect Interfaces

- Connect UART → Ubuntu host (via `minicom`)
- Connect Ethernet → network with DHCP

---

## 5. Boot Procedure (Important)

Power on the board.

The system may fail to boot initially due to the larger image and additional drivers.

You must manually boot from U-Boot.

### 5.1 Manual Boot Commands

Run the following in U-Boot:

mmc info
mmc read 0x90000000 0x100000 0x61A8
setenv fdt_high 0xffffffffffffffff
bootm 0x90000000 - ${fdtcontroladdr}

---

## 6. Network Setup (on target)

After successful boot:

ip link set eth0 up
ip link set eth0 mtu 1200
udhcpc -i eth0
ip addr

---

## 7. Enable SSH Access

echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
echo "PasswordAuthentication yes" >> /etc/ssh/sshd_config

killall sshd
/usr/sbin/sshd

Enable loopback:

ip link set lo up
ip addr add 127.0.0.1/8 dev lo 2>/dev/null || true
ip -6 addr add ::1/128 dev lo 2>/dev/null || true

Set root password:

passwd root

---

## 8. Start MQTT Broker (Optional)

mosquitto -d

---

## 9. Deploy Application

From host (Ubuntu):

scp CloudConnector-riscv root@<BOARD_IP>:/root/

Example:

scp CloudConnector-riscv root@192.168.68.105:/

---

## 10. Run Demo

### Using aicas MQTT Server

./CloudConnector-riscv --token=<TOKEN> -s tcp://demo-jamaicaedg.aicas.com:1883 -n 40 -f 1

The token must be requested from aicas.

### Using Public MQTT Broker

./CloudConnector-riscv -s tcp://test.mosquitto.org:1883 -n 40 -f 1

### Using Local Broker

./CloudConnector-riscv -s tcp://127.0.0.1:1883 -n 40 -f 1

---

## 11. Help

./CloudConnector-riscv --help

---

## 12. Expected Outcome

- Device connects to MQTT broker
- Messages are sent periodically

---

## 13. Optional: Run on PC

./CloudConnector-pc -s tcp://test.mosquitto.org:1883 -n 40 -f 1
