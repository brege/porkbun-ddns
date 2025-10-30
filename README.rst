porkbun-ddns
============

A simple DDNS client for porkbun.com_

.. _porkbun.com: https://porkbun.com

Setup
-----

Clone the repo.

.. code-block:: bash

    sudo mkdir -p /opt/porkbun-ddns
    sudo chown -R user:user /opt/porkbun-ddns
    cd /opt/porkbun-ddns && mkdir /opt/porkbun-ddns/logs
    git clone https://github.com/brege/porkbun-ddns.git .

Create a config file.

.. code-block:: ini

    dns_porkbun_key=pk1_****
    dns_porkbun_secret=sk1_****
    domain=example.com
    data_dir=/opt/porkbun-ddns/logs/

Install as a systemd timer to runs at a 5 minute interval and updates the A records on porkbun.com_.

.. code-block:: bash

    sudo cp porkbun-ddns.* /etc/systemd/system/
    sudo systemctl daemon-reload
    sudo systemctl enable --now porkbun-ddns.timer

Let's Encrypt
-------------

If you use certbot_ from `Let's Encrypt`_, you can use the same config for your renewals.

.. code-block:: bash

    sudo certbot certonly --authenticator dns-porkbun \
      --dns-porkbun-credentials /opt/porkbun-ddns/config.ini \
      --dns-porkbun-propagation-seconds 30 \
      -d example.com -d "*.example.com"

To change an existing renewal entry manually, edit
``/etc/letsencrypt/renewal/example.com.conf``
and change the ``dns_porkbun_credentials =`` entry.

.. _certbot: https://certbot.readthedocs.io

.. _Let's Encrypt: https://letsencrypt.org


Licence
-------
GPLv3_

.. _GPLv3: https://www.gnu.org/licenses/gpl-3.0.en.html

