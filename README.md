# dotfiles-core

> Public GitOps monorepo for cross-platform system and user configuration.

## Overview

`dotfiles-core` contains:
- **common/**: OS-agnostic scripts, templates, and utilities  
- **os/**: Platform-specific folders (linux, macos, windows-wsl, freebsd, openwrt, cisco-ios, mikrotik-routeros, mobile)  
- **environments/**: Host or group overrides  
- **tools/**:
  - `deploy.sh`: Boot-time GitOps deploy script  
  - `ci/`: GitHub Actions or GitLab CI definitions  

## Getting Started

1. Clone the repo as root or a management user:
`sudo git clone git@github.com:yourorg/dotfiles-core.git /opt/dotfiles-core`

2. Make the deploy script executable:
`sudo chmod +x /opt/dotfiles-core/tools/deploy.sh`

3. Add an SSH deploy key at `/root/.ssh/id_deploy` (read-only).
4. Enable automatic deploy at boot/login:
- **Systemd**:
  ```
  sudo tee /etc/systemd/system/deploy-core.service <<EOF
  [Unit]
  Description=Deploy dotfiles-core
  After=network-online.target

  [Service]
  Type=oneshot
  ExecStart=/opt/dotfiles-core/tools/deploy.sh
  User=root

  [Install]
  WantedBy=multi-user.target
  EOF

  sudo systemctl daemon-reload
  sudo systemctl enable deploy-core.service
  ```
- **Cron/macOS/WSL**:
  `sudo crontab -l | { cat; echo "@reboot /opt/dotfiles-core/tools/deploy.sh"; } | sudo crontab -`
  Or source in `/etc/profile`.

## Repository Structure

```
dotfiles-core/
├── .gitignore
├── LICENSE
├── README.md
├── docs/
├── common/
│ ├── profile.d/
│ ├── bin/
│ └── templates/
├── os/
│ ├── linux/
│ ├── macos/
│ ├── windows-wsl/
│ ├── freebsd/
│ ├── openwrt/
│ ├── cisco-ios/
│ ├── mikrotik-routeros/
│ └── mobile/
├── environments/
│ └── <host-name>/
├── tools/
│ ├── deploy.sh
│ └── ci/
└── .github/
└── workflows/
```

## Contributing

Please open issues or submit pull requests. Follow the conventions in `docs/CONTRIBUTING.md`.

## License

This repository is licensed under the Apache-2.0 License. See `LICENSE` for details.

## Templates and Initial Files

.gitignore:
```
/tools/deploy.sh
/os/**/private-*.key
*.swp
.DS_Store
```

License: LICENSE with Apache-2.0

CONTRIBUTING.md: Guidelines for PRs, code style, and testing.
