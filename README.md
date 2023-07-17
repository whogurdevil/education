# This repo is for version control of education app 

## Setup
### These are the guidelines for installing frappe and this app on docker container

### 1. Bootstrap Containers for development

Clone and change directory to frappe_docker directory

```shell
git clone https://github.com/frappe/frappe_docker.git
cd frappe_docker
```

Copy example devcontainer config from `devcontainer-example` to `.devcontainer`

```shell
cp -R devcontainer-example .devcontainer
```

Copy example vscode config for devcontainer from `development/vscode-example` to `development/.vscode`. This will setup basic configuration for debugging.

```shell
cp -R development/vscode-example development/.vscode
```

### 2. Download and Install VS code extention
[VSCode Remote - Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers).

### 3. Setup frappe framework version 14 bench set `PYENV_VERSION` environment variable to `3.10.5` (default) and use NodeJS version 16 (default),
```shell
set environment versions explicitly
nvm use v16
PYENV_VERSION=3.10.12 bench init --skip-redis-config-generation --frappe-branch version-14 frappe-bench
# Switch directory
cd frappe-bench
```

### 4. Add set the values in `common_site_config.json` manually.

```json
{
  "db_host": "mariadb",
  "redis_cache": "redis://redis-cache:6379",
  "redis_queue": "redis://redis-queue:6379",
  "redis_socketio": "redis://redis-socketio:6379"
}
```

### 5. Add a site
```shell
bench --site mysite.localhost set-config developer_mode 1
bench --site mysite.localhost clear-cache
```

### 6. Install ERPNext
Version 14 ERPNext app and is a separate app. Which can only be installed on a fresh site

```shell
bench get-app --branch version-14 --resolve-deps erpnext
bench --site mysite.localhost install-app erpnext
```

### 7. Create SSH Key for making your life easy
```shell
ssh-keygen -t rsa -b 4096 -C "domgesh@cheemspur.com"
```
(this is domgesh's email id please enter you own)

### 8. Copy the key from domgesh@domgesh~/.ssh to frappe@42069~/.ssh 
Copy the keys in your frappe-bench folder and from there copy the keys to ./ssh of your docker continer. Give appropriate permissions to keys using:
```shell
chmod 600 ./id*
```

### 9. Get this repo using bench
```shell
bench get-app --branch main git@github.com:whogurdevil/education
```

### 10. Install app on your website
```shell
bench --site mysite.localhost install-app education
```