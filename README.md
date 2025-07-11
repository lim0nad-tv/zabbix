# zabbix Здаание 1
# Установка Zabbix Server и Agent на PostgreSQL (Debian 11)

Ниже приведён полный набор команд, использованный для установки и настройки Zabbix с базой данных PostgreSQL.

---

## 📦 Установка зависимостей и добавление репозитория

```bash
sudo apt update
sudo apt install -y wget gnupg2 lsb-release
wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_6.0-4+debian11_all.deb
sudo dpkg -i zabbix-release_6.0-4+debian11_all.deb
sudo apt update
```

---

## 🧰 Установка Zabbix Server, Frontend и Agent

```bash
sudo apt install -y zabbix-server-pgsql zabbix-frontend-php zabbix-apache-conf zabbix-agent
```

---

## 🛠 Установка PostgreSQL и создание базы данных

```bash
sudo apt install -y postgresql
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix
```

---

## 📥 Импорт структуры и данных Zabbix

```bash
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
```

---

## 🔧 Настройка Zabbix Server

```bash
sudo nano /etc/zabbix/zabbix_server.conf
```

Внести изменения:

```
DBHost=localhost
DBName=zabbix
DBUser=zabbix
DBPassword=<пароль_К_Пользователю>
```

---

## 🚀 Запуск и автозагрузка сервисов

```bash
sudo systemctl restart zabbix-server zabbix-agent apache2
sudo systemctl enable zabbix-server zabbix-agent apache2
```

---

## 🌍 Доступ к веб-интерфейсу

Откройте:

```
http://192.168.10.14/zabbix
```

Для первого входа используйте:
- **Username**: Admin  
- **Password**: zabbix

---

## ✅ Установка завершена

Теперь Zabbix Server работает на PostgreSQL, агент запущен и сервер доступен через веб.
<img width="1078" height="885" alt="image" src="https://github.com/user-attachments/assets/d34cfc62-9037-41eb-ba14-acba9e2f5c42" />
<img width="1333" height="583" alt="image" src="https://github.com/user-attachments/assets/9af429e5-f92d-4298-b49d-5356d1c6e361" />

# zabbix Задание 2


# Установка Zabbix Agent (клиента) на Ubuntu (версия 6.4)

Этот гайд описывает, как установить и настроить Zabbix Agent версии 6.4 на Ubuntu для подключения к серверу Zabbix.

---

## 1. Добавление репозитория Zabbix и обновление пакетов

    wget https://repo.zabbix.com/zabbix/6.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.4-1+ubuntu$(lsb_release -rs)_all.deb
    sudo dpkg -i zabbix-release_6.4-1+ubuntu$(lsb_release -rs)_all.deb
    sudo apt update

---

## 2. Установка Zabbix Agent

    sudo apt install -y zabbix-agent

---

## 3. Настройка агента

Открой файл конфигурации:

    sudo nano /etc/zabbix/zabbix_agentd.conf

Отредактируй следующие параметры:

    Server=192.168.10.14
    ServerActive=192.168.10.14
    Hostname=Ubuntu-agent

- `Server` — IP или DNS сервера Zabbix, которому разрешён доступ к агенту.  
- `ServerActive` — IP или DNS сервера для активных проверок.  
- `Hostname` — имя хоста, которое совпадает с именем хоста в веб-интерфейсе Zabbix.

---

## 4. Запуск и автозапуск агента

    sudo systemctl restart zabbix-agent
    sudo systemctl enable zabbix-agent

---

## 5. Проверка статуса агента

    sudo systemctl status zabbix-agent

---

## 6. Проверка работы агента

На сервере Zabbix можно проверить связь командой:

    zabbix_get -s <IP_агента> -k agent.ping

Если вывод будет `1` — агент доступен.

---

## Примечание

- Убедитесь, что порты 10050 (агент) и 10051 (сервер) открыты и доступны по сети.  
- Веб-интерфейс Zabbix должен содержать хост с именем, указанным в параметре `Hostname`.

---

Если нужны дополнительные инструкции — обращайтесь!

<img width="3223" height="558" alt="image" src="https://github.com/user-attachments/assets/32383ef5-07b0-417a-9e3e-134cafae40a0" />
<img width="1293" height="480" alt="image" src="https://github.com/user-attachments/assets/c4871abe-91b5-4ec7-b1c0-9b22978ca4a2" />
<img width="3146" height="1369" alt="image" src="https://github.com/user-attachments/assets/4682b0b4-e826-4078-9e41-88d5750f1cff" />
