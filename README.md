# Raspi Basic Configuration

## Requirements

- `sudo apt install sshpass`

## How to use

### Step 1: Install Raspi

- Download the latest Rasbian Version from <https://www.raspberrypi.org/software/operating-systems/>

- Install .img File on SD Card

```bash
sudo dd if=~/Downloads/2020-12-02-raspios-buster-armhf-lite.img of=/dev/sda status=progress
```

- Mount SD Card

```bash
mkdir /tmp/sd
sudo mount /dev/sda1 /tmp/sd
```

- Copy and edit WIFI Config (If needed)

```bash
cp ./files/wpa_supplicant.conf /tmp/sd/
nano /tmp/sd/wpa_supplicant.conf
```

- Add file to enable ssh

```bash
touch /tmp/sd/ssh
```

- Unmount SD Card

```bash
sudo umount /dev/sda1
```

- Insert SD Card to Raspi and Boot

### Step 2: Fill Inventory and  Vars

...

### Step 3: Basic Config

Run the Ansible-Playbooks in the right order

1. Install Galaxy-Requirements from `requirements-collections.yml`

    ```bash
    ansible-galaxy collection install -r requirements-collections.yml
    ```

2. Create the new User defined in `vars/user.yml`

    ```bash
    ansible-playbook 01-create-remote-user.yml
    ```

3. Update the System and install needed Packages defined in `vars/system.yml`

    ```bash
    ansible-playbook 02-update-and-install-software.yml
    ```

4. Add Docker Containers defined in `03-add-containers.yml`

    ```bash
    ansible-playbook 04-add-containers.yml
    ```

## Ports

| Anwendung     | Protocol      | Port  |
|---------------|---------------|-------|
| Docker Swag   | HTTPS         | 443 ¹ |
| PiHole        | HTTP          | 8080 ²|
| Dokuwiki      | HTTP          | 8081  |
| Grafana       | HTTP          | 8082  |
| MeTube        | HTTP          | 8083  |
| Code-Server   | HTTP          | 8084  |
| CAdvisor      | HTTP          | 8085  |
| Fileserver    | HTTP          | 8086  |
| Portainer     | HTTP          | 8087  |
| Prometheus    | HTTP          | 8088  |
| GitLab        | SSH           | 8089  |
| GitLab        | HTTP          | 8090  |
| GitLab        | HTTPS         | 8091  |

¹ : Docker Swag öffnet auch Port 80 (HTTP), leitet diesen aber auf Port 443 (HTTPS) weiter.

² : PiHole öffnet auch die Ports 53 (TCP und UDP) sowie 67 um DNS-Request anzunehmen und zu beantworten.
