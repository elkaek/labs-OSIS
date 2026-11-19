# Лабораторная работа №2: Управление системой и пользователями

## 1 Создание пользователей и групп

Создал 3 новых пользователей admin, user1, user2 с помощью команды useradd опция -m создаёт домашний каталог, -s /bin/bash назначает командную оболочку и задали им пароли командой `passwd`

```bash
useradd -m -s /bin/bash admin
useradd -m -s /bin/bash user1
useradd -m -s /bin/bash user2

passwd admin
passwd user1
passwd user2
```

<img width="801" height="488" alt="Снимок экрана 2026-11-20 035115" src="https://github.com/user-attachments/assets/917678fb-5a92-4e5c-8c50-4fdb64269c66" />
Создание пользователей и установка паролей

Создал 2 группы developers и testers командой groupadd и распределили по ним пользователей с помощью usermod -aG -a добавляет в дополнительную группу, не удаляя из основной. Состав групп проверил командой getent group

```bash
groupadd developers
groupadd testers

usermod -aG developers admin
usermod -aG developers user1
usermod -aG testers user2

getent group developers
getent group testers
```

<img width="493" height="377" alt="Снимок экрана 2026-11-20 035739" src="https://github.com/user-attachments/assets/120eb91a-f76c-4081-b3e9-f69b2dd0c103" />
Создание групп и проверка состава
