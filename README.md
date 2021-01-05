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

1. Create the new User for the Raspi defined in `vars/user.yml`

    ```bash
    ansible-playbook 01-create-remote-user.yml
    ```

2. Remove the 'pi' User and Group

    ```bash
    ansible-playbook 02-remove-pi-user.yml
    ```

3. Update the System and install needed Packages defined in `vars/system.yml`

    ```bash
    ansible-playbook 03-update-and-install-software.yml
    ```
