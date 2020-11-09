# Homelab

Привет! Ты попал на мой мануал по настройке своего личного домашнего сервера. Данный текст я пишу и обновляю лично для себя, как эдакая шпаргалка.

Многое из текста может вам не пригодиться, так что не стоит его рассматривать как исчерпывающий гайд который решит все ваши проблемы.

##### АХТУНГ! - Полный тест на данный момент еще готов. Возможны ошибки, и прочие подводные камни.

## Первоначальная настройка Proxmox VM 

После установки Proxmox VE на железо, первым делом вылезет уведомление об отсутствии подписки на платный репозиторий. Для решение этой проблемы надо произвести следующие действия:

[Скриншот ошибки](https://github.com/zhdanovichq/homelab/raw/main/pic/subscription_server.png)

Отключаем Proxmox от платного репозитория

```shell
vim /etc/apt/sources.list.d/pve-enterprise.list
```

Документируем строчку

```
#deb https://enterprise.proxmox.com/debian/pve buster pve-enterprise
```

Обновляем список репозиториев и обновляем систему

```shell
echo "deb http://download.proxmox.com/debian/pve stretch pve-no-subscription" > /etc/apt/sources.list.d/pve-install-repo.list
```

```shell
wget http://download.proxmox.com/debian/proxmox-ve-release-5.x.gpg -O /etc/apt/trusted.gpg.d/proxmox-ve-release-5.x.gpg
```

```shell
apt update && apt upgrade -y
apt dist-upgrade
```

Отключаем оповещение о отсутствии подписки:

```shell
sed -i "s/getNoSubKeyHtml:/getNoSubKeyHtml_:/" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js
```

## Первоначальная настройка Ubuntu Server

После установки виртуальной машины с убунтой, можно приступиться к первичной конфигурации

 Обновление системы

```shell
sudo apt update 
sudo apt upgrade
```

Инсталяция стартер-пак пакетов
```shell
sudo apt install -y vim zsh mosh tmux htop git jq mc curl wget unzip zip bash gcc build-essential make neofetch apparmor-utils apt-transport-https apt-transport-https ca-certificates dbus network-manager socat software-properties-common supervisor gnupg2 gawk ctop tree python3 python3-venv python3-pip python3-dev golang nodejs npm  php-cli ruby-full bc
```



### Инсталяция и конфигурация **oh-my-zsh** 

Установка oh-my-zsh

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

Конфигурация oh-my-zsh

```
vim ~/.zshrc
    ZSH_THEME="maran"
```

Запуск шела zsh по умолчанию

```shell
chsh -s $(which zsh)
```



### Конфигурация SSH 

Передаем публичный ключ на сервер (из главной машины)

```shell
ssh-copy-id my_user@10.0.x.x
```

Запрещаем вход по паролю

```shell
sudo vim /etc/ssh/sshd_config
    AllowUsers my_user
    PermitRootLogin no
    PasswordAuthentication no
```

Ребут SSH 

```shell
sudo service ssh restart
```

##### 

##Установка **Docker** и **Docker Compose** 

------

Добавляем GPG ключ Docker

```shell
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

Подключаем репозиторий Docker

```shell
sudo add-get-repository \ "deb [arch=amd64] https://download.docker.com/linux/ubuntu \ $(lsb_release -cs) \ stable"
```

 Обновляем репозитории и ставим Docker 

```shell
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
```

Обновляем pip и ставим Docker compose

```shell
sudo pip3 install --upgrade pip
sudo pip3 install docker-compose Обновляем pip и ставим Docker compose
```

В дальнейшем все действия будем выполнять от нашего пользователя **my_user** (1000:1000). Добавляем юзера в группу **docker**, и логинимся в учетку занаво. 

```shell
sudo usermod -a -G docker my_user && su my_user
```
