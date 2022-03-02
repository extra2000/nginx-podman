Production Deployment
=====================

Example how to deploy NGINX for production environment.

Clone repositories
------------------

Execute the following command to clone ``nginx-podman`` repository into your ``~/extra2000`` directory:

.. code-block:: bash

    mkdir ~/extra2000
    cd ~/extra2000
    git clone https://github.com/extra2000/nginx-podman.git

Then, ``cd`` into project root directory:

.. code-block:: bash

    cd nginx-podman

Create NGINX HTTPS OpenSSL Cert
-------------------------------

From the project root directory, ``cd`` into ``deployment/production/nginx``:

.. code-block:: bash

    cd deployment/production/nginx

Create certificate configs:

.. code-block:: bash

    cp -v secrets/certs/configs/nginx-ca-crt.conf{.example,}
    cp -v secrets/certs/configs/nginx-ca-policy.conf{.example,}
    cp -v secrets/certs/configs/nginx-server-csr.conf{.example,}

.. note::

    You may need to edit ``*conf`` files in ``secrets/certs/configs/``.

``cd`` into ``secrets/certs/`` directory:

.. code-block:: bash

    cd secrets/certs/

Create private key for the ``nginx`` host:

.. code-block:: bash

    openssl genrsa -out output/nginx-server.key 2048

Create CSR for the ``nginx`` host:

.. code-block:: bash

    openssl req -new -key output/nginx-server.key -out output/nginx-server.csr -config configs/nginx-server-csr.conf

Create private key for NGINX CA certificate:

.. code-block:: bash

    openssl genrsa -aes128 -out output/nginx-ca.key 2048

.. note::

    You can use password ``abcde12345`` for testing purpose.

Create nginx CA certificate:

.. code-block:: bash

    openssl req -new -x509 -key output/nginx-ca.key -out output/nginx-ca.crt -config configs/nginx-ca-crt.conf -days 1825

Create ``index.txt`` and ``serial`` files required during signing certificates:

.. code-block:: bash

    touch index.txt
    echo '01' > serial

Sign ``nginx`` certificate:

.. code-block:: bash

    openssl ca -config configs/nginx-ca-policy.conf -out output/nginx-server.crt -infiles output/nginx-server.csr

Verify certificates:

.. code-block:: bash

    openssl verify -CAfile output/nginx-ca.crt output/nginx-server.crt

Upload required certificates to the ``nginx`` host:

.. code-block:: bash

    scp -P 22 ./secrets/certs/output/nginx.crt USER@NGINX-IP:extra2000/nginx-podman/deployment/production/nginx/secrets/certs/output/
    scp -P 22 ./secrets/certs/output/nginx.key USER@NGINX-IP:extra2000/nginx-podman/deployment/production/nginx/secrets/certs/output/

On the ``nginx`` host, make sure the certificates are mountable by Podman:

.. code-block:: bash

    chcon -R -v -t container_file_t secrets/certs/output/

Generate ``dhparam.pem``
------------------------

From the project root directory, ``cd`` into ``deployment/production/nginx``:

.. code-block:: bash

    cd deployment/production/nginx

Execute the following command to generate ``dhparam.pem``:

.. code-block:: bash

    openssl dhparam -dsaparam -out ./secrets/dhparam.pem 2048

.. note::

    The ``-dsaparam`` is used to ensure FIPS compliance.

Deploy NGINX
------------

From the project root directory, ``cd`` into ``deployment/production/nginx``:

.. code-block:: bash

    cd deployment/production/nginx

Create config files:

.. code-block:: bash

    cp -v configmaps/nginx.yaml{.example,}
    cp -v configs/conf.d/default.conf{.example,}
    cp -v configs/conf.d/ssl.conf{.example,}
    cp -v configs/ssl-params.conf{.example,}

Allow the following files to be mounted into container:

.. code-block:: bash

    chcon -R -v -t container_file_t ./secrets ./configs

Create pod file:

.. code-block:: bash

    cp -v nginx-pod.yaml{.example,}

Create SELinux security policy:

.. code-block:: bash

    cp -v selinux/nginx_podman.cil{.example,}

Load SELinux security policy:

.. code-block:: bash

    sudo semodule -i selinux/nginx_podman.cil /usr/share/udica/templates/base_container.cil

Verify that the SELinux module exists:

.. code-block:: bash

    sudo semodule --list | grep -e "nginx_podman"

Deploy NGINX:

.. code-block:: bash

    podman play kube --configmap configmaps/nginx.yaml --seccomp-profile-root ./seccomp nginx-pod.yaml

Test NGINX. Open web-browser and go to either http://localhost or https://localhost

To view logs, execute the following commands:

.. code-block:: bash

    podman exec -it nginx-pod-srv01 tail /var/log/nginx/ssl-access.log
    podman exec -it nginx-pod-srv01 tail /var/log/nginx/ssl-error.log
    podman exec -it nginx-pod-srv01 tail /var/log/nginx/access.log
    podman exec -it nginx-pod-srv01 tail /var/log/nginx/error.log

Create systemd files to run at startup:

.. code-block:: bash

    mkdir -pv ~/.config/systemd/user
    cd ~/.config/systemd/user
    podman generate systemd --files --name nginx-pod
    systemctl --user enable pod-nginx-pod.service container-nginx-pod-srv01.service
