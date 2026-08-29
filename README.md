# Security Policy

## Supported Versions

Please use the latest release of Docker-OSX to ensure you have all security updates and QEMU patches.

| Version | Supported          |
| ------- | ------------------ |
| Latest  | :white_check_mark: |
| < 1.0   | :x:                |

## Reporting a Vulnerability

If you discover a security vulnerability within Docker-OSX, please report it responsibly:

1. Do NOT open a public GitHub issue.
2. Contact the maintainer directly via Twitter [@sickcodes](https://twitter.com/sickcodes) or email `sickcodes@gmail.com`.
3. Provide a proof of concept (PoC) and details regarding your host setup (QEMU version, host OS, kernel version, Docker version).

## Container & Host Security Best Practices

- **KVM Access**: Limit host `/dev/kvm` access strictly to the `kvm` or `libvirt` group rather than granting global read/write permissions.
- **VNC Exposure**: Avoid exposing VNC (default port `5900`) directly to untrusted networks. Use SSH port forwarding (`-L 5900:127.0.0.1:5900`) or a VPN.
- **Least Privilege**: Avoid passing full `--privileged` flags when standard device passthrough (`--device /dev/kvm`) suffices for your container environment.