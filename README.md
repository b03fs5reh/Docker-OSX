#!/usr/bin/env bash

set -euo pipefail

RAM="${RAM:-4}"
SMP="${SMP:-4}"
CORES="${CORES:-2}"
LISTEN_ADDR="${LISTEN_ADDR:-127.0.0.1}"
DISK_IMAGE="${DISK_IMAGE:-mac_hdd_ng.img}"
EXTRA="${EXTRA:-}"

echo "Starting Docker-OSX with ${RAM}G RAM..."

if [ ! -f "${DISK_IMAGE}" ]; then
    echo "Error: Disk image '${DISK_IMAGE}' not found." >&2
    exit 1
fi

qemu-system-x86_64 \
    -enable-kvm \
    -m "${RAM}G" \
    -machine q35,accel=kvm:tcg \
    -smp "${SMP},sockets=1,cores=${CORES},threads=2" \
    -cpu Penryn,kvm=on,vendor=GenuineIntel,+invtsc,vmware-cpuid-freq=on \
    -device isa-applesmc,osk="ourhardworkbythesewordsguardedpleasedontsteal(c)AppleComputerInc" \
    -drive if=pflash,format=raw,readonly=on,file=OVMF_CODE.fd \
    -drive if=pflash,format=raw,file=OVMF_VARS-1024x768.fd \
    -smbios type=2 \
    -device ich9-intel-hda -device hda-output \
    -drive id=MacHDD,if=none,file="${DISK_IMAGE}",format=qcow2 \
    -device ide-hd,bus=sata.2,drive=MacHDD \
    -netdev user,id=net0,hostfwd=tcp:${LISTEN_ADDR}:50922-:22,hostfwd=tcp:${LISTEN_ADDR}:5900-:5900 \
    -device vmxnet3,netdev=net0,id=net0,mac=52:54:00:c9:18:27 \
    -monitor stdio \
    ${EXTRA}