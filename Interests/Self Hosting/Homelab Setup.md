# Flake.nix
```nix
# flake.nix
{
  description = "NixOS configuration with modular services";

  inputs = {
    nixpkgs.url = "github:NixOS/nixpkgs/nixos-unstable";
    nixos-mailserver = {
      url = "gitlab:simple-nixos-mailserver/nixos-mailserver";
      inputs.nixpkgs.follows = "nixpkgs";
    };
  };

  outputs = { self, nixpkgs, nixos-mailserver, ... }: {
    nixosConfigurations = {
      # VPS Configuration
      vps = nixpkgs.lib.nixosSystem {
        system = "x86_64-linux";
        modules = [
          ./hardware-configuration.nix
          ./configuration.nix
          ./modules/mail-server.nix
          ./modules/reverse-proxy.nix
          ./modules/authelia.nix
          ./modules/security.nix
          nixos-mailserver.nixosModules.mailserver
        ];
      };
      
      # Proxmox VM Configuration
      proxmox-vm = nixpkgs.lib.nixosSystem {
        system = "x86_64-linux";
        modules = [
          ./hardware-configuration-vm.nix
          ./configuration-vm.nix
          ./modules/reverse-proxy.nix
          ./modules/authelia.nix
          ./modules/security.nix
        ];
      };
    };
  };
}
```

# Authelia.nix
```nix
# modules/authelia.nix
{ config, pkgs, ... }:

{
  services.authelia.instances.main = {
    enable = true;
    
    settings = {
      # Server configuration
      server = {
        host = "127.0.0.1";
        port = 9091;
        path = "";
        asset_path = "";
      };

      # Logging
      log = {
        level = "info";
        format = "text";
      };

      # Theme
      theme = "auto";
      
      # JWT secret (generate with: openssl rand -hex 32)
      jwt_secret = "your-jwt-secret-here-replace-this-with-random-32-hex-chars";
      
      # Default redirection URL
      default_redirection_url = "https://www.wurt.net";
      
      # TOTP settings
      totp = {
        issuer = "wurt.net";
        period = 30;
        skew = 1;
      };

      # Authentication backend
      authentication_backend = {
        password_reset.disable = false;
        file = {
          path = "/var/lib/authelia-main/users_database.yml";
          password = {
            algorithm = "argon2id";
            iterations = 1;
            salt_length = 16;
            parallelism = 8;
            memory = 64;
          };
        };
      };

      # Access Control
      access_control = {
        default_policy = "deny";
        rules = [
          {
            domain = "auth.wurt.net";
            policy = "bypass";
          }
          {
            domain = "app.wurt.net";
            policy = "two_factor";
          }
          # Add more rules for your applications
        ];
      };

      # Session configuration
      session = {
        name = "authelia_session";
        domain = "wurt.net";
        same_site = "lax";
        expiration = "1h";
        inactivity = "5m";
        remember_me_duration = "1M";
        
        # Redis configuration (using local Redis)
        redis = {
          host = "127.0.0.1";
          port = 6379;
          database_index = 0;
        };
      };

      # Regulation (brute force protection)
      regulation = {
        max_retries = 3;
        find_time = "2m";
        ban_time = "5m";
      };

      # Storage (local SQLite)
      storage = {
        local = {
          path = "/var/lib/authelia-main/db.sqlite3";
        };
      };

      # Notifier (SMTP via your mail server)
      notifier = {
        disable_startup_check = false;
        smtp = {
          host = "127.0.0.1";
          port = 587;
          timeout = "5s";
          username = "admin@wurt.net";
          password = "your-mail-password";
          sender = "admin@wurt.net";
          identifier = "wurt.net";
          subject = "[Authelia] {title}";
          startup_check_address = "admin@wurt.net";
        };
      };
    };
  };

  # Enable Redis for session storage
  services.redis.servers.authelia = {
    enable = true;
    port = 6379;
    bind = "127.0.0.1";
  };

  # Create users database file
  environment.etc."authelia/users_database.yml" = {
    text = ''
      users:
        admin:
          displayname: "Admin User"
          password: "$argon2id$v=19$m=65536,t=3,p=4$hash-here"  # Generate with authelia hash-password
          email: admin@wurt.net
          groups:
            - admins
            - dev
        # Add more users as needed
    '';
    mode = "0600";
  };

  # System user for authelia
  users.users.authelia-main = {
    isSystemUser = true;
    group = "authelia-main";
    home = "/var/lib/authelia-main";
    createHome = true;
  };
  
  users.groups.authelia-main = {};

  # Ensure proper permissions
  systemd.services.authelia-main.serviceConfig = {
    StateDirectory = "authelia-main";
    StateDirectoryMode = "0700";
  };
}
```

# Security.nix
```nix
# modules/security.nix
{ config, pkgs, ... }:

{
  # Fail2ban for SSH protection
  services.fail2ban = {
    enable = true;
    maxretry = 3;
    ignoreIP = [
      "127.0.0.0/8"
      "10.0.0.0/8"
      "192.168.0.0/16"
    ];
    
    jails = {
      ssh = {
        enabled = true;
        port = "ssh";
        filter = "sshd";
        logpath = "/var/log/auth.log";
        maxretry = 3;
        bantime = "10m";
      };
      
      nginx-http-auth = {
        enabled = true;
        port = "http,https";
        filter = "nginx-http-auth";
        logpath = "/var/log/nginx/error.log";
        maxretry = 5;
        bantime = "10m";
      };
    };
  };

  # Automatic security updates
  system.autoUpgrade = {
    enable = true;
    allowReboot = false;  # Set to true if you want automatic reboots
    channel = "https://nixos.org/channels/nixos-unstable";
  };

  # Kernel hardening
  boot.kernel.sysctl = {
    # IP Spoofing protection
    "net.ipv4.conf.default.rp_filter" = 1;
    "net.ipv4.conf.all.rp_filter" = 1;
    
    # IP packet forwarding
    "net.ipv4.ip_forward" = 0;
    
    # Ignore ICMP redirects
    "net.ipv4.conf.all.accept_redirects" = 0;
    "net.ipv6.conf.all.accept_redirects" = 0;
    
    # Ignore send redirects
    "net.ipv4.conf.all.send_redirects" = 0;
    
    # Disable source packet routing
    "net.ipv4.conf.all.accept_source_route" = 0;
    "net.ipv6.conf.all.accept_source_route" = 0;
    
    # Log Martians
    "net.ipv4.conf.all.log_martians" = 1;
    
    # TCP SYN flood protection
    "net.ipv4.tcp_syncookies" = 1;
    
    # Ignore ping requests
    "net.ipv4.icmp_echo_ignore_all" = 1;
  };

  # Additional security packages
  environment.systemPackages = with pkgs; [
    ufw
    lynis
    chkrootkit
    rkhunter
  ];

  # Sudo configuration
  security.sudo = {
    enable = true;
    wheelNeedsPassword = true;
  };

  # Disable root login completely
  users.users.root.hashedPassword = "!";
}
```

# Setup.sh
```bash
# scripts/setup.sh
#!/usr/bin/env bash
# Helper script for initial setup

set -e

echo "NixOS Setup Helper Script"
echo "========================="

# Generate mail password hash
echo "Generating mail server password hash..."
echo "Please enter your mail password:"
read -s MAIL_PASSWORD
MAIL_HASH=$(echo "$MAIL_PASSWORD" | mkpasswd -s)
echo "Mail password hash: $MAIL_HASH"
echo "Add this to your mail-server.nix configuration"

# Generate Authelia password hash
echo ""
echo "Generating Authelia password hash..."
echo "Please enter your Authelia admin password:"
read -s AUTH_PASSWORD
# You'll need to install authelia to generate this hash
echo "Run this command after deploying: authelia hash-password '$AUTH_PASSWORD'"

# Generate JWT secret
echo ""
echo "Generating JWT secret for Authelia..."
JWT_SECRET=$(openssl rand -hex 32)
echo "JWT Secret: $JWT_SECRET"
echo "Add this to your authelia.nix configuration"

# Generate session secret
echo ""
echo "Generating session secret..."
SESSION_SECRET=$(openssl rand -hex 32)
echo "Session Secret: $SESSION_SECRET"

echo ""
echo "Setup complete! Remember to:"
echo "1. Update domain names in all configuration files"
echo "2. Add your SSH public key to configuration.nix"
echo "3. Update IP addresses for your local network"
echo "4. Create DNS records for your domains"
echo "5. Run 'sudo nixos-rebuild switch --flake .#vps' on your VPS"
echo "6. Run 'sudo nixos-rebuild switch --flake .#proxmox-vm' on your VM"
```

# Deploy-vps.sh
```bash
# scripts/deploy-vps.sh
#!/usr/bin/env bash
# Deploy to VPS

set -e

echo "Deploying to VPS..."
sudo nixos-rebuild switch --flake .#vps

echo "Creating mail password file..."
sudo mkdir -p /var/lib/dovecot
echo "Enter mail password for admin@wurt.net:"
read -s password
echo "$password" | mkpasswd -s | sudo tee /var/lib/dovecot/admin-password
sudo chmod 600 /var/lib/dovecot/admin-password

echo "VPS deployment complete!"
```

# Deploy-vm.sh
```bash
# scripts/deploy-vm.sh
#!/usr/bin/env bash
# Deploy to Proxmox VM

set -e

echo "Deploying to Proxmox VM..."
sudo nixos-rebuild switch --flake .#proxmox-vm

echo "VM deployment complete!"
```

# README.md
# NixOS Modular Configuration

This repository contains a modular NixOS configuration for running services across a VPS and local Proxmox VM setup.

## Structure

```
.
├── flake.nix                    # Main flake configuration
├── configuration.nix            # VPS configuration
├── configuration-vm.nix         # Proxmox VM configuration
├── hardware-configuration*.nix  # Hardware configurations
├── modules/
│   ├── mail-server.nix         # Email server with nixos-mailserver
│   ├── reverse-proxy.nix       # Nginx reverse proxy
│   ├── authelia.nix           # Authentication server
│   └── security.nix           # Security hardening
└── scripts/
    ├── setup.sh               # Initial setup helper
    ├── deploy-vps.sh          # VPS deployment
    └── deploy-vm.sh           # VM deployment
```

## Services

### VPS Services
- **Mail Server**: Full-featured email server with DKIM, DMARC, spam filtering
- **Reverse Proxy**: Nginx with SSL termination and security headers
- **Authelia**: 2FA authentication for protected applications
- **Security**: Fail2ban, automatic updates, kernel hardening

### Proxmox VM Services
- **Reverse Proxy**: Local nginx for internal service routing
- **Authelia**: Authentication for local applications
- **Security**: Basic hardening for internal network

## Setup Instructions

1. **Clone and customize**:
```bash
git clone <this-repo>
cd nixos-config
```
2. **Update configurations**:
   - Replace `wurt.net` with your actual domain
   - Update IP addresses in `configuration-vm.nix`
   - Add your SSH public key to both configurations
   - Update email addresses

3. **Generate secrets**:
```bash
chmod +x scripts/setup.sh
./scripts/setup.sh
```

4. **Create DNS records**:
   - `A` record: `wurt.net` → VPS IP
   - `A` record: `www.wurt.net` → VPS IP
   - `A` record: `mail.wurt.net` → VPS IP
   - `A` record: `auth.wurt.net` → VPS IP
   - `A` record: `app.wurt.net` → VPS IP
   - `MX` record: `wurt.net` → `mail.wurt.net`
   - `TXT` record: DKIM (generated after mail server deployment)
   - `TXT` record: SPF: `v=spf1 mx ~all`
   - `TXT` record: DMARC: `v=DMARC1; p=quarantine; rua=mailto:admin@wurt.net`

5. **Deploy to VPS**:
```bash
./scripts/deploy-vps.sh
```

6. **Deploy to VM**:
```bash
./scripts/deploy-vm.sh
```

## Post-Deployment

1. **Test mail server**:
```bash
# Check mail server status
systemctl status postfix dovecot rspamd

# Test SMTP
telnet localhost 25
```

2. **Configure Authelia users**:
```bash
# Generate password hash
authelia hash-password "your-password"

# Update /etc/authelia/users_database.yml
```

3. **Set up DKIM**:
The mail server will generate DKIM keys automatically. Add the public key to your DNS:
```bash
cat /var/dkim/wurt.net.mail.txt
```

## Adding New Protected Applications

1. Add a new virtual host in `modules/reverse-proxy.nix`:
```nix
"newapp.wurt.net" = {
 enableACME = true;
 forceSSL = true;
 locations."/authelia" = { /* auth config */ };
 locations."/" = {
   proxyPass = "http://192.168.1.XX:XXXX";
   # ... auth and proxy config
 };
};
```

2. Add access control rule in `modules/authelia.nix`:
```nix
{
 domain = "newapp.wurt.net";
 policy = "two_factor";
}
```

3. Create DNS record and redeploy.

## Troubleshooting

- **Mail delivery issues**: Check logs with `journalctl -u postfix -f`
- **SSL certificate issues**: Check ACME logs with `journalctl -u acme-wurt.net -f`
- **Authelia issues**: Check logs with `journalctl -u authelia-main -f`
- **Proxy issues**: Check nginx logs in `/var/log/nginx/`

## Migration from nginx-proxy-manager

To migrate from your current LXC setup:
1. Export your proxy configurations
2. Convert them to the nginx format shown in `reverse-proxy.nix`
3. Set up Authelia for applications that need protection
4. Update DNS to point to your new setup
5. Test thoroughly before decommissioning the old setup

## Configuration.nix
```nix
# configuration.nix (VPS)
{ config, pkgs, ... }:

{
  imports = [
    ./hardware-configuration.nix
    ./modules/mail-server.nix
    ./modules/reverse-proxy.nix
    ./modules/authelia.nix
    ./modules/security.nix
  ];

  # Basic system configuration
  boot.loader.systemd-boot.enable = true;
  boot.loader.efi.canTouchEfiVariables = true;

  networking = {
    hostName = "nixos-vps";
    networkmanager.enable = true;
    firewall = {
      enable = true;
      allowedTCPPorts = [ 22 80 443 25 587 993 ];
      allowedUDPPorts = [ ];
    };
  };

  # Time zone and locale
  time.timeZone = "Europe/Amsterdam";
  i18n.defaultLocale = "en_US.UTF-8";

  # User configuration
  users.users.admin = {
    isNormalUser = true;
    extraGroups = [ "wheel" "networkmanager" ];
    openssh.authorizedKeys.keys = [
      # Add your SSH public key here
	  "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDLxYWRdvzd01GebtUTxUgLO6F5z/ricceiy6Gs6IU/H pinkfloyd@terra.local"
    ];
  };

  # Essential packages
  environment.systemPackages = with pkgs; [
    neovim
    curl
    wget
    git
    htop
    certbot
  ];

  # SSH configuration
  services.openssh = {
    enable = true;
    settings = {
      PermitRootLogin = "no";
      PasswordAuthentication = false;
    };
  };

  system.stateVersion = "24.05";
}
```

## Mail-server.nix
```nix
# modules/mail-server.nix
{ config, pkgs, ... }:

{
  mailserver = {
    enable = true;
    fqdn = "mail.wurt.net";  # Replace with your domain
    domains = [ "wurt.net" ];
    
    # Login accounts
    loginAccounts = {
      "admin@wurt.net" = {
        hashedPasswordFile = "/var/lib/dovecot/admin-password";
        aliases = [ "postmaster@wurt.net" ];
      };
    };

    # SSL certificates
    certificateScheme = "acme-nginx";
    
    # Enable services
    enableImap = true;
    enableImapSsl = true;
    enablePop3 = false;
    enablePop3Ssl = false;
    enableSubmission = true;
    enableSubmissionSsl = true;
    
    # Spam filtering
    enableRspamd = true;
    rspamd.extraConfig = ''
      extended_spam_headers = true;
    '';
    
    # Enable virus scanning
    enableClamav = true;
    
    # Backup MX
    backup = true;
    
    # Additional security
    dkimSigning = true;
    dmarcReporting = {
      enable = true;
      domain = "wurt.net";
    };
  };

  # ACME configuration for Let's Encrypt
  services.nginx.enable = true;
  security.acme = {
    acceptTerms = true;
    defaults.email = "admin@wurt.net";
  };

  # Create password file (run this manually after deployment)
  # echo "your-password" | mkpasswd -s | sudo tee /var/lib/dovecot/admin-password
}
```

## Reverse-proxy.nix
```nix
# modules/reverse-proxy.nix
{ config, pkgs, ... }:

{
  services.nginx = {
    enable = true;
    
    # Recommended settings
    recommendedGzipSettings = true;
    recommendedOptimisation = true;
    recommendedProxySettings = true;
    recommendedTlsSettings = true;
    
    # Common configuration
    commonHttpConfig = ''
      # Hide nginx version
      server_tokens off;
      
      # Security headers
      add_header X-Frame-Options DENY;
      add_header X-Content-Type-Options nosniff;
      add_header X-XSS-Protection "1; mode=block";
      add_header Referrer-Policy "strict-origin-when-cross-origin";
      
      # Rate limiting
      limit_req_zone $binary_remote_addr zone=login:10m rate=1r/s;
      limit_req_zone $binary_remote_addr zone=api:10m rate=10r/s;
    '';

    virtualHosts = {
      # Main domain redirect to www
      "wurt.net" = {
        serverAliases = [ "wurt.net" ];
        globalRedirect = "www.wurt.net";
        enableACME = true;
        forceSSL = true;
      };

      # Main website
      "www.wurt.net" = {
        enableACME = true;
        forceSSL = true;
        locations."/" = {
          proxyPass = "http://192.168.1.10:8080";  # Your local service
          proxyWebsockets = true;
          extraConfig = ''
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
          '';
        };
      };

      # Authelia authentication
      "auth.wurt.net" = {
        enableACME = true;
        forceSSL = true;
        locations."/" = {
          proxyPass = "http://127.0.0.1:9091";
          extraConfig = ''
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_set_header X-Forwarded-Host $http_host;
            proxy_set_header X-Forwarded-Uri $request_uri;
            proxy_set_header X-Forwarded-Ssl on;
          '';
        };
      };

      # Protected application example
      "app.wurt.net" = {
        enableACME = true;
        forceSSL = true;
        locations."/authelia" = {
          proxyPass = "http://127.0.0.1:9091/api/verify";
          extraConfig = ''
            internal;
            proxy_pass_request_body off;
            proxy_set_header Content-Length "";
            proxy_set_header X-Original-URL $scheme://$http_host$request_uri;
            proxy_set_header X-Original-Method $request_method;
            proxy_set_header X-Forwarded-Method $request_method;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_set_header X-Forwarded-Host $http_host;
            proxy_set_header X-Forwarded-Uri $request_uri;
            proxy_set_header X-Forwarded-For $remote_addr;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header Authorization $http_authorization;
            proxy_pass_request_headers on;
          '';
        };
        locations."/" = {
          proxyPass = "http://192.168.1.11:3000";  # Your protected app
          extraConfig = ''
            auth_request /authelia;
            auth_request_set $target_url $scheme://$http_host$request_uri;
            auth_request_set $user $upstream_http_remote_user;
            auth_request_set $groups $upstream_http_remote_groups;
            auth_request_set $name $upstream_http_remote_name;
            auth_request_set $email $upstream_http_remote_email;
            
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_set_header Remote-User $user;
            proxy_set_header Remote-Groups $groups;
            proxy_set_header Remote-Name $name;
            proxy_set_header Remote-Email $email;
            
            error_page 401 = @error401;
          '';
        };
        locations."@error401" = {
          return = "302 https://auth.wurt.net/?rd=$target_url";
        };
      };
    };
  };

  # Open firewall ports
  networking.firewall.allowedTCPPorts = [ 80 443 ];
  
  # ACME configuration
  security.acme = {
    acceptTerms = true;
    defaults.email = "admin@wurt.net";
  };
}
```

## Configuraiton-vm.nix
```nix
# configuration-vm.nix (Proxmox VM)
{ config, pkgs, ... }:

{
  imports = [
    ./hardware-configuration-vm.nix
    ./modules/reverse-proxy.nix
    ./modules/authelia.nix
    ./modules/security.nix
  ];

  # VM-specific configuration
  boot.loader.grub.enable = true;
  boot.loader.grub.device = "/dev/sda";

  networking = {
    hostName = "nixos-proxy";
    networkmanager.enable = true;
    # Configure static IP for your local network
    interfaces.eth0.ipv4.addresses = [{
      address = "192.168.178.60";  # Adjust to your network
      prefixLength = 24;
    }];
    defaultGateway = "192.168.178.1";  # Your router IP
    nameservers = [ "1.1.1.1" "8.8.8.8" ];
    firewall = {
      enable = true;
      allowedTCPPorts = [ 22 80 443 9091 ];
    };
  };

  time.timeZone = "Europe/Amsterdam";
  i18n.defaultLocale = "en_US.UTF-8";

  users.users.admin = {
    isNormalUser = true;
    extraGroups = [ "wheel" ];
    openssh.authorizedKeys.keys = [
      # Same SSH key as VPS
	  "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDLxYWRdvzd01GebtUTxUgLO6F5z/ricceiy6Gs6IU/H pinkfloyd@terra.local"
    ];
  };

  environment.systemPackages = with pkgs; [
    neovim
    curl
    wget
    git
    htop
  ];

  services.openssh = {
    enable = true;
    settings = {
      PermitRootLogin = "no";
      PasswordAuthentication = false;
    };
  };

  system.stateVersion = "24.05";
}
```

# Setup Process
I can definitely help you set up a comprehensive NixOS configuration for your VPS and proxmox setup! Let me create a modular structure that covers your mail server, reverse proxy, and authentication needs.Now let me create the modular service configurations:Now let me add some helper scripts and documentation:Perfect! I've created a comprehensive modular NixOS configuration that addresses all your requirements. Here's what this setup provides:

## Key Features

**🏗️ Modular Architecture**: Clean separation of concerns with dedicated modules for each service

- `mail-server.nix` - Complete email server with nixos-mailserver
- `reverse-proxy.nix` - Nginx with SSL, security headers, and Authelia integration
- `authelia.nix` - 2FA authentication server with Redis session storage
- `security.nix` - Hardening with fail2ban, kernel tuning, and automatic updates

**📧 Mail Server**: Full-featured setup with:

- DKIM signing and DMARC reporting
- Spam filtering (Rspamd) and virus scanning (ClamAV)
- IMAP/SMTP with SSL/TLS
- Automatic SSL certificates via ACME

**🔐 Security & Authentication**:

- Authelia for 2FA protection of web applications
- Fail2ban for intrusion prevention
- Security headers and rate limiting
- SSH key-only authentication

**🔄 Dual Configuration**:

- VPS config with mail server and internet-facing services
- Proxmox VM config for local network proxy services

## Quick Start

1. **Customize the configuration**:
    - [x] Replace all instances of `yourdomain.com` with your domain
    - [x] Update IP addresses in `configuration-vm.nix` for your network
    - [x] Add your SSH public key to both configurations
2. **Generate secrets**:
```bash
chmod +x scripts/setup.sh
./scripts/setup.sh
```
3. **Set up DNS records** (A, MX, DKIM, SPF, DMARC as detailed in the README)
4. **Deploy**:
```bash
# On VPS
sudo nixos-rebuild switch --flake .#vps

# On Proxmox VM  
sudo nixos-rebuild switch --flake .#proxmox-vm
```
