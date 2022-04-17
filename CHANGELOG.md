# Changelog

## [3.0.0](https://github.com/extra2000/nginx-podman/compare/v2.0.0...v3.0.0) (2022-04-17)


### ⚠ BREAKING CHANGES

* pod file need to mount stream configs
* **configs:** pod now requires `nginx.conf` to be mounted

### Features

* add stream for TCP proxy ([8b723f9](https://github.com/extra2000/nginx-podman/commit/8b723f9d34c58b477351066fc0edfa5f47d6cf18))
* **configs:** externalize `nginx.conf` as volume ([5f4ffba](https://github.com/extra2000/nginx-podman/commit/5f4ffba6e55eadce9df319a862b43c43679d5887))


### Fixes

* **ssl-params:** fix `X-Frame-Options` issues ([dc473fe](https://github.com/extra2000/nginx-podman/commit/dc473fe72358b1bc6140f29f151cf0e7f55ca475))


### Documentations

* **configs:** add example proxy configs for Kibana ([ac18961](https://github.com/extra2000/nginx-podman/commit/ac189618c588d19fdd5c593e8674ebcd4750beba))
* **configs:** add example proxy for `cockpit` ([18e6ea2](https://github.com/extra2000/nginx-podman/commit/18e6ea2fadc8be6e5fcd449ce467eb1fe9d21265))
* **configs:** add IPv6 ([3cee644](https://github.com/extra2000/nginx-podman/commit/3cee644bef77e8975fffd0f0bdaba65c66b97cdc))
* **deployment:** add instructions for SSL params stream and SSH reverse proxy example ([b2b1599](https://github.com/extra2000/nginx-podman/commit/b2b159920393cb3f568847a59d8df4f1e4faa664))
* **deployments:** add instructions for `nginx.conf` ([a2775ec](https://github.com/extra2000/nginx-podman/commit/a2775ec245dac2139ebcf6547e580944771ff368))
* **deployment:** simplify podman systemd ([ab09c79](https://github.com/extra2000/nginx-podman/commit/ab09c7925644d27064ddca85a3efc70d677ea245))
* **host-preparations:** change `grubby` to `/etc/default/grub` method ([1e53ec5](https://github.com/extra2000/nginx-podman/commit/1e53ec55edccf37e9fa41acbda1b1e2f5d654610))

## [2.0.0](https://github.com/extra2000/nginx-podman/compare/v1.0.0...v2.0.0) (2022-03-02)


### ⚠ BREAKING CHANGES

* **selinux:** SELinux policy name has changed from `nginx` to `nginx_podman`
* **selinux:** Users are now required to create their own SELinux policy based on the given example

### Documentations

* **deployments:** add instructions to create SELinux policy ([d1eb1f0](https://github.com/extra2000/nginx-podman/commit/d1eb1f05c12566b65f5ad250ba1e454f82bf17fc))


### Code Refactoring

* **selinux:** change SELinux from `nginx` to `nginx_podman` to avoid conflict with non-podman `nginx` SELinux policy from host ([24cd260](https://github.com/extra2000/nginx-podman/commit/24cd26029e319587e391fd2aa3beb5966f6034a6))
* **selinux:** make SELinux policy as example file ([ca7c149](https://github.com/extra2000/nginx-podman/commit/ca7c149faacc51faf0582ccc8c6d1b527e078abb))

## 1.0.0 (2022-02-11)


### Features

* initial implementations ([21163f2](https://github.com/extra2000/nginx-podman/commit/21163f22e968a70b1550714dbfcf7ae9c5b3369b))


### Documentations

* **README:** update `README.md` ([ccb9177](https://github.com/extra2000/nginx-podman/commit/ccb9177ebdefb2b6fe11707ace6cfc27b299796a))


### Continuous Integrations

* add AppVeyor with `semantic-release` ([da35811](https://github.com/extra2000/nginx-podman/commit/da3581140da95936b105d1b24bb01a6472ab2c1f))
