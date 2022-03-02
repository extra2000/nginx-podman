# Changelog

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
