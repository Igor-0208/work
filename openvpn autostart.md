INIT
===

1. 'nano /etc/init.d/openvpn'

            # OpenVPN autostart on boot script

            start on runlevel [2345]
            stop on runlevel [!2345]

            respawn

            exec /usr/sbin/openvpn --status /var/run/openvpn.client.status 10 --cd /etc/openvpn --config /etc/openvpn/client.conf --syslog openvpn



SYSTEMD

1. Скачиваем файл конфигурации example.ovpn
2. sudo mv ~/example.ovpn /etc/openvpn/example.conf 
3. sudo vim /etc/openvpn/example.txt
            # добавляем логин и пароль
            YOUR_VPN_USERNAME
            YOUR_VPN_PASSWORD
            # сохраняем
4. sudo vim /etc/openvpn/example.conf
            auth-user-pass /etc/openvpn/example.txt
5. Проверяем sudo openvpn --config /etc/openvpn/example.conf

настройка systemd

1. sudo vim /usr/lib/systemd/system/openvpn@example.service

            [Unit]
            Description=OpenVPN Robust And Highly Flexible Tunneling Application On example
            After=syslog.target network.target

            [Service]
            PrivateTmp=true
            Type=forking
            PIDFile=/var/run/openvpn/example.pid
            ExecStart=/usr/sbin/openvpn --daemon --writepid /var/run/openvpn/example.pid --cd /etc/openvpn/ --config example.conf

            [Install]
            WantedBy=multi-user.target

2. sudo systemctl start openvpn@example.service

3. sudo systemctl status openvpn@example.service



