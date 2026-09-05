# Security Policy

## Reporting Security Issues

Please report any security vulnerabilities responsibly rather than opening a public issue.

- **Contact:** Reach out to the maintainers directly or submit via GitHub Private Vulnerability Reporting.
- **Report Details:** Include a brief description, PoC or steps to reproduce, and container configuration details.

## Security & Threat Model Considerations

- **Exposed Ports:** By default, exposed VNC (5900) and SSH (2222/22) ports should not be exposed to untrusted networks without updating default credentials.
- **Host KVM Access:** Access to `/dev/kvm` allows direct hardware virtualization. Ensure container permissions are appropriate when running in multi-tenant environments.