# Man

📟 Привет. Меня зовут [NikolasMelui][nikolasmelui], я занимаюсь разработкой. В последнее время стал замечать, что с регулярностью примерно раз в 2-3 недели гуглю то, что делаю редко, но делаю. Чтобы перестать искать то, что уже находил, решил сделал свой мануал, в котором, со временем, будет всё. Пока на русском, но скоро будет перевод.

## VPS

### SSH_LOCAL

- На локальной машине заходим в

```bash
cd ~/.ssh
```

- Создаем новый ssh ключ для нашего сервера

```bash
ssh-keygen -t rsa -b 4096
```

- Запускаем ssh-agent (если собираемся использовать passphrase)

```bash
eval“$(ssh-agent -s)”
```

- Добавляем наш ключ в агента

```bash
ssh-add -K ~/.ssh/curkey_rsa
```

- Добавляем в конфигурационный файл настройки подключения к серверу

```bash
vim ~/.ssh/config
```

### SSH_REMOTE

- Заходим на сервер по ssh от root юзера через ввод пароля

```bash
ssh root@yourserveradress.com
```

- Генерируем SSH ключ (для доступа в GitLab)

```bash
ssh-keygen
```

- Добавляем нового юзера с именем manager (чтобы не работать из под рута)

```bash
adduser manager
```

  > Если что-то пошло не так, удаляем юзера manager и пересоздаем его

  ```bash
  deluser manager
  adduser manager
  ```

  > В случае необходимости меняем пароль

  ```bash
  passwd manager
  ```

- Добавляем юзера manager в группу админов

```bash
usermod -aG sudo manager
```

- Проверяем юзера

```bash
id manager
```

- Логинемся за нашего юзера через sudo

```bash
sudo su - manager
```

- Создаем юзеру папку для ssh

```bash
sudo mkdir ~/.ssh
```

- Делаем ей нужные права

```bash
chmod 700 ~/.ssh
```

- Создаем файл для авторизованных ключей

```~/.bash
ssh/authorized_keys
```

- Заходим на локальную машину и копируем наш публичный SSH ключ

```bash
ssh-copy-id -i ~/.ssh/projectname_manager_rsa.pub manager@yourserveradress
```

- Меняем права доступа на файл для авторизованных ключей

```bash
chmod 600 ~/.ssh/authorized_keys
```

- Перезагружаем ssh службу

```bash
sudo service ssh restart
```

- Выходим из сессии юзера и сессии рута, отключаемся от сервера и пробуем зайти по ssh с использованием ключа
- Заходим в конфиг файл и закрываем доступ для рута и возможность заходить на сервер через введение пароля

```bash
sudo nano /etc/ssh/sshd_config
```

  > PermitRootLogin no
  > PasswordAuthentication no

- Перезагружаем наш sshd процесс

```bash
sudo systemctl reload sshd
```

#### License

MIT License

Copyright (c) 2018 NikolasMelui

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

**From developers 2 developers.**
[NikolasMelui][nikolasmelui]

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)
   [nikolasmelui]: <https://github.com/NikolasMelui>
