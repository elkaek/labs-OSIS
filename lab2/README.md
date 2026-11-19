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

## 2 Настройка прав доступа

Создал каталоги /home/admin/data и /home/admin/logs, назначил владельцев (chown) и права доступа (chmod)
- **/home/admin/data** права 700, владелец admin:admin (редактировать файлы может только admin).
- **/home/admin/logs** права 770, владелец admin:developers (участники группы developers могут читать и записывать файлы).
- **/home/admin** права 755, это чтобы группа developers могла зайти внутрь в подкаталог logs.

```bash
mkdir -p /home/admin/data /home/admin/logs
chown admin:admin /home/admin/data
chown admin:developers /home/admin/logs
chmod 700 /home/admin/data
chmod 770 /home/admin/logs
chmod 755 /home/admin
ls -la /home/admin
```

<img width="691" height="397" alt="Снимок экрана 2026-11-20 041248" src="https://github.com/user-attachments/assets/f04118af-e5af-4d86-a47a-96ca68f81428" />


Настройка прав доступа к каталогам
