# proftpd

1. роль только для debian 10
2. для включения sftp необходимо сгенерировать хост ключ:

`ssh-keygen -f /tmp/proftpd_ssh_host_rsa_key -t rsa -N '' -m PEM`

ключ зашифровать и положить в переменную proftpd_ssh_host_rsa_key

3. Для генерации хэша пароля можно вызвать ``ftpasswd --hash`` (ввести пароль и в консоль оплучите хэш), либо использовать online генератор htpasswd с md5 алгоритмом, например - https://www.askapache.com/online-tools/htpasswd-generator/

Пример запуска playbook:
```bash
- hosts: "all"
  become: yes
  roles:
    - proftpd
  vars:
    proftpd_users:
      - { username: 'example1', passwd_hash: '$1$QKW8OMFY$YRYOvvlyUHnXmJV9NqI8Q1' }
      - { username: 'example2', passwd_hash: '$1$YUnPqQEQ$zQY6S/zr7NvsS65d/AWFc.', pub_keys: "{{ example2_pub_keys }}" }

    example2_pub_keys:
      - |
        AAAAB3NzaC1yc2EAAAABJQAAAQBSRp0uNXk43jIfj3zumJ0iMaDNui+ZxJss7lZ5
        0IFkDegRv/sVt2EsSBvG7ccKwfZRu/Uvtgt2J/6ML43YeKZFj5QD+4uL6QCQ/BJ6
        7Bi8f+FtnalIZHSg9NF8viabo1Hto3k/ZCzLu9tPzxzIHbXm5jmGI/nHGEVJo8Hg
        WY6FOAvyiRiQc33rhKjU73ZgIgMtq3V0xj8wDtOo2xa9+0nCTPZyuezUd/2BxiPC
        9ugU3BIWeE2BT1Bqvq48oPi2f+xxsQASdWOcQ5xv4gBapr5XlFP69WlJ5M0rnbrk
        aM0tLA9ZkZJQT6NAfYWhLHelim0f8NU6sPawM9ZtHgSOQ5pb
      - |
        AAAAB3NzaC1yc2EAAAABJQAAAQBSRp0uNXk43jIfj3zumJ0iMaDNui+ZxJss7lZ5
        0IFkDegRv/sVt2EsSBvG7ccKwfZRu/Uvtgt2J/6ML43YeKZFj5QD+4uL6QCQ/BJ6
        7Bi8f+FtnalIZHSg9NF8viabo1Hto3k/ZCzLu9tPzxzIHbXm5jmGI/nHGEVJo8Hg
        WY6FOAvyiRiQc33rhKjU73ZgIgMtq3V0xj8wDtOo2xa9+0nCTPZyuezUd/2BxiPC
        9ugU3BIWeE2BT1Bqvq48oPi2f+xxsQASdWOcQ5xv4gBapr5XlFP69WlJ5M0rnbrk
        aM0tLA9ZkZJQT6NAfYWhLHelim0f8NU6sPawM9ZtHgSOQ5pb
```

В результате будет создано два пользователя FTP.
example1 - авторизация только по паролю
example2 - авторизация по паролю и по ключу 
