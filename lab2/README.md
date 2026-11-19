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

## 3 Команды управления пользователями

Изученные команды:

useradd создание пользователя
usermod изменение учётной записи
groupadd создание группы
passwd смена пароля
chown смена владельца или группы файла
chmod смена прав доступа

Сменил пароль user1, заблокировал user2 `usermod -L`, проверил статус командой `passwd -S` буква L учётная запись заблокирована, затем разблокал `usermod -U` статус P пароль активен

```bash
passwd user1
usermod -L user2
passwd -S user2
usermod -U user2
passwd -S user2
```

<img width="319" height="243" alt="Снимок экрана 2026-11-20 043035" src="https://github.com/user-attachments/assets/7f79a28d-d303-45f6-b103-39f654495d7e" />


Смена пароля и блокировка пользователя

## 4 Мониторинг активности пользователей

who кто находится в системе сейчас (имя, терминал, время входа)
w то же самое, + ято он сейчас делает  (средняя загрузка и текущая команда каждого пользователя)
last история входов в систему из журнала

В результате изучил
```bash
who
w
last
```

<img width="794" height="362" alt="Снимок экрана 2026-11-20 044303" src="https://github.com/user-attachments/assets/6364f52e-b517-4c3a-87bb-7e24b263e650" />


Мониторинг активности пользователей

## 5 Автоматизация управления пользователями

Создал текстовый файл staff.txt со списком пользователей и их целевых групп AM, Arthas, Papanya и написали bash-скрипт bananza_bum.sh, который построчно читает файл, пропускает пустые строки, проверяет существование группы (getent group) и пользователя, а потом автоматически создаёт недостающие учётные записи и добавляет их в соответствующие группы

staff.txt
```
AM developers
Arthas testers
Papanya developers
```
bananza_bum.sh
```bash
#!/bin/bash

FILE="staff.txt"

while read user grp; do
    [ -z "\$user" ] && continue

    # тут создаём группу если ещё нет
    if ! getent group "\$grp" > /dev/null; then
        groupadd "\$grp"
        echo "группа \$grp заспавнилась"
    fi

    # создаю пользователя если его ещё нет
    if id "\$user" > /dev/null 2>&1; then
        echo "пользователь \$user уже живёт"
    else
        useradd -m -s /bin/bash -G "grp" "user"
        echo "пользователь user успешно проявился (группа grp)"
    fi
done < "\$FILE"
```

<img width="623" height="405" alt="Снимок экрана 2026-11-20 051839" src="https://github.com/user-attachments/assets/8106bc74-207c-4df1-ac54-f832f7aa1407" />


Код bananza_bum.sh

Сделал скрипт исполняемым (chmod +x bananza_bum.sh), запустил его и проверил состав групп developers и testers командой getent group

```
chmod +x bananza_bum.sh
cat staff.txt
./bananza_bum.sh
getent group developers testers
```

<img width="437" height="300" alt="Снимок экрана 2026-11-20 052151" src="https://github.com/user-attachments/assets/3c967ea7-4e44-4b53-b118-39ec68a70c1b" />


Запуск скрипта и проверка состава групп

## Вывод

В ходе выполнения лабы были изучены базовые инструменты администрирования пользователей, групп и прав доступа в ОС Linux (Debian)
Освоено создание учётных записей через useradd, управление группами groupadd, usermod -aG, смена паролей и блок/разблок пользователей (passwd, usermod -L/-U)
Настроено разграничение прав доступа к каталогам с помощью команд chown и chmod
Изучены утилиты мониторинга активности пользователей в системе (who, w, last)
Разработан bash-скрипт с проверками существования объектов (getent, id) для авто массового создания пользователей и групп из текстового файла
