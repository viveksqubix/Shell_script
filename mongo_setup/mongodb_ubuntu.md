- To determine which Ubuntu release your host is running, run the following command on the host's terminal:
```sh
cat /etc/lsb-release
```

# Install MongoDB Community Edition
## Import the public key used by the package management system
- From a terminal, install gnupg and curl if they are not already available:
```sh
sudo apt-get install gnupg curl
```
- To import the MongoDB public GPG key, run the following command:
```sh
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor
```

## Create a list file for MongoDB
- Create the list file /etc/apt/sources.list.d/mongodb-org-7.0.list for your version of Ubuntu(22.04).
```sh
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```

## Reload local package database
- Issue the following command to reload the local package database:
```sh
sudo apt-get update
```

## Install the MongoDB packages
- To install the latest stable version, issue the following
```sh
sudo apt-get install -y mongodb-org
```

- Optional. Although you can specify any available version of MongoDB, apt-get will upgrade the packages when a newer version becomes available. To prevent unintended upgrades, you can pin the package at the currently installed version:

```sh
echo "mongodb-org hold" | sudo dpkg --set-selections
echo "mongodb-org-database hold" | sudo dpkg --set-selections
echo "mongodb-org-server hold" | sudo dpkg --set-selections
echo "mongodb-mongosh hold" | sudo dpkg --set-selections
echo "mongodb-org-mongos hold" | sudo dpkg --set-selections
echo "mongodb-org-tools hold" | sudo dpkg --set-selections
```

## Data Directory for MongoDB
- If you installed through the package manager, the data directory /var/lib/mongodb and the log directory /var/log/mongodb are created during the installation.

## Configuration File
- The official MongoDB package includes a configuration file (/etc/mongod.conf). These settings (such as the data directory and log directory specifications) take effect upon startup. That is, if you change the configuration file while the MongoDB instance is running, you must restart the instance for the changes to take effect.

## If you are unsure which init system your platform uses, run the following command:

```sh
ps --no-headers -o comm 1
```

Then select the appropriate tab below based on the result:
- systemd - select the systemd (systemctl) tab below.
- init - select the System V Init (service) tab below.

## Command to run/stop/restart mongodb

```sh
sudo systemctl start mongod
```

- If you receive an error similar to the following when starting mongod:
```sh
Failed to start mongod.service: Unit mongod.service not found.
```

Run the following command first:
```sh
sudo systemctl daemon-reload
```
```sh
sudo systemctl stop mongod
sudo systemctl restart mongod
# Remove MongoDB databases and log files.
sudo rm -r /var/log/mongodb
sudo rm -r /var/lib/mongodb
```