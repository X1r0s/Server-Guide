````markdown
# Руководство по настройке сервера и управлению им для Node.js проектов

Это руководство предназначено для пользователей, которые хотят настроить и управлять Linux-сервером для развертывания Node.js проектов. В гайде описаны шаги по обновлению системы, базовые команды Linux, управление пользователями, настройка фаервола, установка необходимых компонентов и автоматизация деплоя.

---

## Оглавление

- [1. Настройка сервера (Linux)](#1-настройка-сервера-linux)
  - [1.1. Обновление системы](#11-обновление-системы)
  - [1.2. Основные команды Linux](#12-основные-команды-linux)
  - [1.3. Управление пользователями и правами](#13-управление-пользователями-и-правами)
  - [1.4. Настройка фаервола (UFW)](#14-настройка-фаервола-ufw)
- [2. Установка необходимых компонентов](#2-установка-необходимых-компонентов)
  - [2.1. Установка Node.js и npm](#21-установка-nodejs-и-npm)
  - [2.2. Установка Git](#22-установка-git)
  - [2.3. Установка PM2](#23-установка-pm2)
- [3. Мониторинг и логирование](#3-мониторинг-и-логирование)
  - [3.1. Мониторинг системных ресурсов](#31-мониторинг-системных-ресурсов)
  - [3.2. Просмотр логов приложений](#32-просмотр-логов-приложений)
- [4. Автоматизация деплоя](#4-автоматизация-деплоя)
  - [4.1. Использование Git-хуков (post-receive)](#41-использование-git-хуков-post-receive)
  - [4.2. Настройка CI/CD с GitHub Actions](#42-настройка-cicd-с-github-actions)
- [Заключение](#заключение)

---

## 1. Настройка сервера (Linux)

### 1.1. Обновление системы

Перед началом работы с сервером рекомендуется обновить список пакетов и систему:

```bash
sudo apt update
sudo apt upgrade -y
```
````

### 1.2. Основные команды Linux

Вот несколько базовых команд, которые помогут вам ориентироваться и работать с файловой системой:

- **Навигация:**
  - `cd` – сменить директорию  
    _Пример:_ `cd /var/www`
  - `ls` – вывести список файлов и папок  
    _Пример:_ `ls -la`
- **Работа с файлами и папками:**
  - `cp` – копировать файлы или папки  
    _Пример:_ `cp file.txt /path/to/destination/`
  - `mv` – переместить или переименовать файлы  
    _Пример:_ `mv oldname.txt newname.txt`
  - `rm` – удалить файл или папку  
    _Пример:_ `rm file.txt` или `rm -r folder/`
  - `mkdir` – создать новую директорию  
    _Пример:_ `mkdir new_folder`
- **Управление правами:**
  - `chmod` – изменить права доступа к файлам  
    _Пример:_ `chmod 755 script.sh`
  - `chown` – изменить владельца файла  
    _Пример:_ `chown user:group file.txt`

### 1.3. Управление пользователями и правами

Некоторые команды для создания пользователей и управления правами:

- `sudo` – выполнение команд с правами суперпользователя  
  _Пример:_ `sudo apt update`
- `adduser` – создание нового пользователя  
  _Пример:_ `sudo adduser username`
- `usermod` – изменение параметров пользователя  
  _Пример:_ `sudo usermod -aG sudo username`

### 1.4. Настройка фаервола (UFW)

Для защиты сервера можно использовать UFW:

1. Установите UFW (если не установлен):
   ```bash
   sudo apt install ufw -y
   ```
2. Разрешите доступ по SSH:
   ```bash
   sudo ufw allow ssh
   ```
3. Разрешите необходимые порты (например, для веб-сервера):
   ```bash
   sudo ufw allow 80/tcp
   sudo ufw allow 443/tcp
   ```
4. Включите UFW:
   ```bash
   sudo ufw enable
   ```
5. Проверьте статус:
   ```bash
   sudo ufw status
   ```

---

## 2. Установка необходимых компонентов

### 2.1. Установка Node.js и npm

Установите Node.js (рекомендуется LTS-версия) и npm:

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
```

Проверьте установку:

```bash
node -v
npm -v
```

### 2.2. Установка Git

Git позволяет управлять версиями вашего кода.

```bash
sudo apt install git -y
```

Проверьте версию:

```bash
git --version
```

### 2.3. Установка PM2

PM2 – это процесс-менеджер для Node.js приложений, который помогает:

- Автоматически перезапускать приложение при сбоях.
- Просматривать логи и состояние процессов.
- Обеспечивать автозапуск при перезагрузке сервера.

```bash
npm install -g pm2
```

Проверьте установку:

```bash
pm2 -v
```

---

## 3. Мониторинг и логирование

### 3.1. Мониторинг системных ресурсов

Для мониторинга состояния сервера используйте следующие команды:

- `top` – выводит список процессов в реальном времени.
- `htop` – улучшенная версия top (может потребоваться установка: `sudo apt install htop`).
- `free -h` – проверка использования оперативной памяти.
- `df -h` – проверка использования дискового пространства.

### 3.2. Просмотр логов приложений

Если вы используете PM2, для просмотра логов вашего приложения:

```bash
pm2 logs my-app
```

Для очистки логов:

```bash
pm2 flush
```

---

## 4. Автоматизация деплоя

### 4.1. Использование Git-хуков (post-receive)

Git-хук позволяет автоматически обновлять код на сервере при получении новых коммитов.

1. Перейдите в папку `.git/hooks` вашего репозитория на сервере:
   ```bash
   cd /path/to/your/project/.git/hooks
   ```
2. Создайте или отредактируйте файл `post-receive`:
   ```bash
   sudo nano post-receive
   ```
3. Вставьте следующий скрипт:
   ```bash
   #!/bin/bash
   # Определяем рабочую директорию
   GIT_WORK_TREE=/path/to/your/project git pull origin main
   cd /path/to/your/project
   npm install
   pm2 restart my-app
   echo "✅ Автоматический деплой выполнен!"
   ```
4. Сохраните файл и сделайте его исполняемым:
   ```bash
   sudo chmod +x post-receive
   ```

### 4.2. Настройка CI/CD с GitHub Actions

Вы также можете автоматизировать деплой с помощью GitHub Actions:

1. Создайте в корне проекта папку `.github/workflows/` и файл `deploy.yml`.
2. Пример содержимого `deploy.yml`:

   ```yaml
   name: Deploy Node.js App

   on:
     push:
       branches:
         - main

   jobs:
     deploy:
       runs-on: ubuntu-latest

       steps:
         - name: Checkout repository
           uses: actions/checkout@v2

         - name: Deploy to Server via SSH
           uses: appleboy/ssh-action@v0.1.5
           with:
             host: ${{ secrets.SERVER_IP }}
             username: ${{ secrets.SERVER_USER }}
             key: ${{ secrets.SSH_PRIVATE_KEY }}
             script: |
               cd /path/to/your/project
               git pull origin main
               npm install
               pm2 restart my-app
   ```

3. Добавьте секретные переменные в GitHub в разделе **Settings → Secrets and variables → Actions**:
   - `SERVER_IP` – IP-адрес вашего сервера.
   - `SERVER_USER` – имя пользователя (например, `root`).
   - `SSH_PRIVATE_KEY` – приватный SSH-ключ для подключения к серверу.

---

God Save The JS!

```

```
