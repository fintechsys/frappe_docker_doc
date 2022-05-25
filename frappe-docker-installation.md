### To build
```sh
sudo apt install docker
sudo apt install docker-compose
sudo usermod -a -G docker <username>
git clone https://github.com/frappe/frappe_docker.git
cd frappe_docker
cp -R devcontainer-example .devcontainer
docker-compose -f .devcontainer/docker-compose.yml up -d
docker-compose -f .devcontainer/docker-compose.yml exec --user frappe frappe bash -c "sudo chown -R frappe:frappe ."
```

### To Run Docker:
```sh
docker-compose exec -e "TERM=xterm-256color" -w /workspace/development frappe bash
```

### To build frappe_env
> After  Run the docker **execute** the follows:

  ```sh
  bench init --skip-redis-config-generation --frappe-branch version-13 frappe-bench
  ```
#### setup hosts
> in the frappe_bench folder

- set mariadb host to container running mariadb
```sh
bench set-mariadb-host mariadb
```

> if show errors then try
```sh
bench set-mariadb-host mariad
nano sites/common_site_config.json
```
> and change `"db_host": "mariad"` to `"db_host": "mariadb"`

- set redis hosts
```sh
bench set-redis-cache-host redis-cache:6379
bench set-redis-queue-host redis-queue:6379
bench set-redis-socketio-host redis-socketio:6379
```

### create new sites
> in the frappe_bench folder

```sh
bench new-site <sitename> --no-mariadb-socket
```

> site name should end with `.localhost`

> default password mariadb root user  `123`

### enable developer mode
> in the frappe_bench folder

```sh
bench --site <sitename> set-config developer_mode 1
bench --site <sitename> clear-cache
```

----------------------------------------------------------------

### Reference
- <https://github.com/frappe/frappe_docker/tree/main/development>
