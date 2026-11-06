# Домашнее задание: «Защита сети»

**Студент:** Захарян Николай Артурович  
**Дата:** 07.11.2026
---
### Защищаемая система (192.168.56.10):
```bash
sudo apt update
sudo apt install suricata fail2ban openssh-server -y
sudo suricata-update
sudo systemctl enable --now suricata fail2ban
```
## Система злоумышленника (192.168.56.11):
```bash
sudo apt update
sudo apt install nmap thc-hydra -y
```
## Задание 1: Разведка системы (Nmap)
Выполненные команды на системе злоумышленника (192.168.56.11):
```bash
sudo nmap -sA 192.168.56.10
sudo nmap -sT 192.168.56.10
sudo nmap -sS 192.168.56.10
sudo nmap -sV 192.168.56.10
```
<img width="692" height="268" alt="2026-11-06_12-40-35" src="https://github.com/user-attachments/assets/3516dfdf-cd7a-4e6b-ba43-0298988c699f" />
<img width="694" height="243" alt="2026-11-06_12-40-58" src="https://github.com/user-attachments/assets/3d025aa9-0e34-4767-86a2-c10c7a6ddc21" />
<img width="697" height="243" alt="2026-11-06_12-41-12" src="https://github.com/user-attachments/assets/bc8f7e5d-48b9-48d5-be12-259cb5f9ae2d" />
<img width="691" height="290" alt="2026-11-06_12-41-37" src="https://github.com/user-attachments/assets/77addfbd-76d4-419e-8838-d37837c8c89c" />

## Задание 2: Атака подбора пароля SSH
1. Подготовка к атаке (Система злоумышленника 192.168.56.11):
```bash
echo -e "admin\nroot\nuser\ntest" > users.txt
echo -e "123456\npassword\nqwerty\nadmin123" > pass.txt
```
2. Настройка Fail2Ban (Защищаемая система 192.168.56.10):
Создан файл /etc/fail2ban/jail.local:

[sshd]
enabled = true
port = ssh
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
bantime = 600
findtime = 600
```bash
sudo systemctl restart fail2ban
```
3. Выполнение атаки:
```bash
hydra -L users.txt -P pass.txt 192.168.56.10 ssh
```
<img width="1436" height="574" alt="2026-11-06_12-56-50" src="https://github.com/user-attachments/assets/c926737e-cb65-4de1-a1de-2c59d8b631f8" />
