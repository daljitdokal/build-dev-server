# dev-server
Build server so that we can deploy and test new application

## Step 1: Install Debain

- Download Debian: https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-12.9.0-amd64-netinst.iso
- Create bootable pendrive with rufus
- Boot
  - Plugin into device and boot the machine
  - Make sure to add network cable for internet
  - Choose GUI to install debian
  - Make sure to select `sshd` installation for remote acces

## Step 2: Server Default Setup

- Add `authorized_keys`
  ```bash
  mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys
  echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDETXf0BqcX1UzKUibGpsV+tSzFhKx7fH/8y/zs2Yjdv1LOyHcc7yQM8ICNY3nfhl2czJfqL66JhTz/WG2OE+PIJLMa7cOoGXM+UgGZ5EMxnTpRgJBSbBrxjGTjKxvK5RkFVx9gFjfmrCEBOy52AzRvnr2UPx21ZBCVJougwpthE39Wo9eOVY66Dz1k6dMSH82dcr4l9WjZpFHj93V5NZnKSHEvJqFQb1E7mslST93lJ+yedgit8DsNToVADKAj4qCtbqU8QWAR+SNCEhNLXY0TJrxeEDDdryvDvPHpJSsfdYx3eN8caMusg9cfKeAUa7PckznnPf43tgr4D/stCeBsq2PSSTAKCiJu9P3uSYYZfFzOiF/G+3mB49csCzMYRMWZJMqFaF0BId2ERiCUut8KXKDIjfiGq3BilhZQwhBkeSaHrV6R94sgGpaBhCy6u15FD+zAUjDvkfUElrXjkfzRMFoaOmadhqtm3cIYn+zWhjquVprJIezkbGLboKm2DuPI/1X5Kx9HRlJujb/rUY3UdtiAqyTF7Gt3X5u/sjumyjwfldRrnqGdoUu6yXN+o7eAoF1VygEQKI5yp4rEqJ9dhxY0w/4ECBSqtjwS9eNahKjo5oxfphFkRW316Pu6ws2UiSNUgzAANWBamyslfS4/lgsCBCqMCmB6VW+xpk0vMw== geekywebmaster-laptop" >> ~/.ssh/authorized_keys
  echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQD+QBBGvwHru82xozEKGaahE2/cyqqT9AqvPA57NyjweIVmH3zsvIpxoqRkMAEIejKENTLm/nD+q+n5OFVcKccnj09ifb0A8RzSFp7FzUt7fCH7MZLzyNGYoeOjA49endp2vSXR4B7HZCMDJQ5i2c694/xcNXUDW6Pd/r+LRfZj5YMq2I7fWmOj2gdKgYTbIQD9f5a6e+NOPx4ZiYEGWwT8y7BgrXLxYgPGBes4K8aFsT7jsWxmyhTo2TvEXPo1lMThW9jr7sSp0e9y5Ufx+SYhjqoKknN9yc/CzQFdAjic+K/tmLCdw8gMuJa8YAPkDceOz/FD5SC0gLgSSspGiIOZ geekywebmaster-desktop" >> ~/.ssh/authorized_keys
  ```
- Run `sudo usermod -aG sudo daljit ` to add the user into the sudo group as root.
- Custom update to `sshd_config` file. (i.e. update default port)
  ```bash
  sudo nano /etc/ssh/sshd_config
  ```
- Restart `sshd` service
  ```bash
  systemctl status sshd
  systemctl restart sshd
  ```
- Install pakages
  ```bash
   sudo apt install htop curl git sed
  ```
- Update `.bashrc` to your default setups
- Get local IP address
 ```bash
# Get Local IP address
nmcli -p device show | grep IP4.ADD | grep 192
```

## Step 3: Remote connection

```bash
ssh daljit@192.168.1.145 -p 22
```

## Step 4: Install `K3S`

```bash
curl -sfL https://get.k3s.io | sh -
```

## Steps 5: Install Applications

--------

### Keycloak

```bash
sudo kubectl create namespace keycloak
sudo kubectl -n keycloak create -f https://raw.githubusercontent.com/keycloak/keycloak-quickstarts/latest/kubernetes/keycloak.yaml

# Wait for deplyment
sudo kubectl -n keycloak get pods -w

# Get port
sudo kubectl -n keycloak get service keycloak -o yaml | grep nodePort

# View in browser
URL: http://192.168.1.145:31074
USER: admin
PASS: admin
```
