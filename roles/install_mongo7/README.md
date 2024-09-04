# Install MongoDB 7 role

This role will install MongoDB 7 Community Edition using the [instructions for installing Community Edition on Ubuntu](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/).

## Intended configuration

This role uses a local tempalate to create the configuration file on the target. Currently, this is a very basic default configuration. This template could be used to drive more advanced configuration of MongoDB.

## Variables

This role does not use any variables. It installs Mongo 7 and uses Ansible facts to determine which Mongo repository to use for the installation.