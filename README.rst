porkbun-ddns
============

A simple DDNS client for porkbun.com_

    I just want the **A** records for **\*.my-domain.xyz** and **my-domain.xyz** to justwerk™.

.. _porkbun.com: https://porkbun.com

Setup
-----

Install the script.

.. code-block:: bash

    git clone https://github.com/brege/porkbun-ddns.git
    cd porkbun-ddns
    sudo install -m 755 porkbun /usr/local/bin/porkbun-ddns

Create a config file.

.. code-block:: bash

    sudo mkdir -p /etc/porkbun-ddns
    sudo mkdir -p /var/lib/porkbun-ddns

.. code-block:: ini

    # /etc/porkbun-ddns/config.ini
    dns_porkbun_key=pk1_****
    dns_porkbun_secret=sk1_****
    domain=example.com
    data_dir=/var/lib/porkbun-ddns

Install as a systemd timer to runs at a 5 minute interval and updates the A records on porkbun.com_.

.. code-block:: bash

    sudo cp systemd/porkbun-ddns.* /etc/systemd/system/
    sudo systemctl daemon-reload
    sudo systemctl enable --now porkbun-ddns.timer

Let's Encrypt
-------------

If you use certbot_ from `Let's Encrypt`_, you can use the same config for your renewals.

.. code-block:: bash

    sudo certbot certonly --authenticator dns-porkbun \
      --dns-porkbun-credentials /etc/porkbun-ddns/config.ini \
      --dns-porkbun-propagation-seconds 30 \
      -d example.com -d "*.example.com"

To change an existing renewal entry manually, edit
``/etc/letsencrypt/renewal/example.com.conf``
and change the ``dns_porkbun_credentials =`` entry.

.. _certbot: https://certbot.readthedocs.io

.. _Let's Encrypt: https://letsencrypt.org


Notes
-----

- The script explicitly filters Porkbun’s response down to ``"type":"A"``
  entries. Everything else (MX, TXT, CNAME, SRV, etc.) is ignored. 
- This scripts' only objective is to ensure your domain and its subdomains are
  always reachable from the internet.

Licence
-------
GPLv3_

.. _GPLv3: https://www.gnu.org/licenses/gpl-3.0.en.html

