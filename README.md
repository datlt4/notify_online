# Setup dev environment

```bash
python3 -m pip install -r requirements.txt
pre-commit install
```

# Export environment variables

```bash
export MAIL_SERVER="smtp.gmail.com"
export MAIL_PORT=587
export MAIL_USERNAME="********************************"
export MAIL_RECEIPIENT="********************************"
export MAIL_PASSWORD="********************************"
export MAIL_USE_TLS=1
export MAIL_USER_SSL=0
```

# Run application

```bash
python3 -c "from notify_online import notify_online; notify_online()"
```

# Build package

```bash
python3 setup.py sdist bdist_wheel
```

# Publish package to Pypi

```bash
twine upload dist/*
```

# Setup daemon

```bash
sudo nano /etc/systemd/system/notify-online.service
```

```yaml
[Unit]
Description=Daemon for serving notify-online
After=network.target

[Service]
User=1000
Group=1000
Environment=MAIL_SERVER="smtp.gmail.com" MAIL_PORT=587 MAIL_USERNAME="................@gmail.com" MAIL_RECEIPIENT="................@gmail.com" MAIL_PASSWORD="............" MAIL_USE_TLS=1 MAIL_USER_SSL=0
ExecStart=/bin/bash -c '/home/emoi/anaconda3/bin/python3 -m pip install --upgrade notify-online && /home/emoi/anaconda3/bin/python3 -c "from notify_online import notify_online; notify_online()"'
ExecReload=/usr/bin/kill -s HUP $MAINPID
Restart=always
# RestartSec=60

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reload
sudo systemctl status notify-online.service
sudo journalctl -u notify-online.service

sudo systemctl start notify-online.service
sudo systemctl restart notify-online.service
sudo systemctl stop notify-online.service

sudo systemctl enable notify-online.service
sudo systemctl is-enabled notify-online.service
sudo systemctl disable notify-online.service
```
