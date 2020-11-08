# Homelab

Привет! Ты попал на мой мануал по настройке своего личного домашнего сервера. Данный текст я пишу и обновляю лично для себя, как эдакая шпаргалка.

Многое из текста может вам не пригодиться, так что не стоит его рассматривать как исчерпывающий гайд который решит все ваши проблемы.

##### АХТУНГ! - Полный тест на данный момент еще готов. Возможны ошибки, и прочие подводные камни.

## Первоначальная настройка Proxmox VM 

После установки Proxmox VE на железо, первым делом вылезет уведомление об отсутствии подписки на платный репозиторий. Для решение этой проблемы надо произвести следующие действия:

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
