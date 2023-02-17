# CollectiveAccess Boilerplate

CollectiveAccess Boilerplate, using Docker containers, with PHP, Apache and MySQL. This repository is solely intended as a starting point for a local development environment for CollectiveAccess implementations. I am not affiliated with any company or products mentioned or used in this repository.

**Work In Progress**. See the [issues page](https://github.com/rahenrique/rah.collectiveaccess.boilerplate/issues) for more information.

## Prerequisites

Before starting, you will need to have the following tools installed on your machine:
* [Git](https://git-scm.com)
* [Docker](https://www.docker.com/)
* [Docker Compose](https://docs.docker.com/compose/install/)

## Installation (Local Development)

```bash
# Clone this repository
$ git clone https://github.com/rahenrique/rah.collectiveaccess.boilerplate.git

# Run the application
$ docker-compose up
```

This will install the two CollectiveAccess applications: [Pawtucket](https://github.com/collectiveaccess/pawtucket2) (the front-end application) and [Providence](https://github.com/collectiveaccess/providence) (the back-end, administrative application)

To access each application:
* Pawtucket: <http://localhost>
* Providence: <http://localhost/providence/>

## Installing a profile
If you try to access the Pawtucket application for the first time, it will tell you that you need a funcioning Providence installation in order to Pawtucket to work.

To do so, just access the Providence application for the first time.
A message will show in the interface, like:

> It looks like you have not installed your database yet. Check your configuration or run the installer.

Click to run the installer, or access <http://localhost/providence/install/>

Select one of the available installation profiles. At the end of this proccess, your database will be created with the corresponding tables and initial data, and a admin user will be created. **Be sure to save the provided login and password after the install**.

## That's it!
See the [official documentation](https://manual.collectiveaccess.org/) to start cataloging your art assets.

## Troubleshooting
### vm.max_map_count is too low
In case your Elasticsearch container fails to build, and you see an error message telling you that `vm.max_map_count is too low`, perhaps you have to run the following command to increase this value:
```bash
sudo sysctl -w vm.max_map_count=262144
```
After that, try to run the container again.
To permanently fix this issue, edit the `/etc/sysctl.conf` file and add the line:
```bash
vm.max_map_count=262144
```

Reference: https://github.com/docker-library/elasticsearch/issues/111

### Elasticsearch not working
The branch currently in use for **Providence** is set as `"dev/graphql-api"` in order to allow testing the GraphQL API implementation. In this Collective Access' Providence branch, the Elasticsearch implementation is broken. To use Elasticsearch, please edit the files: 
`/.docker/providence/Dockerfile`: change the variable `ENV CA_PROVIDENCE_VERSION="dev/graphql-api"` to `ENV CA_PROVIDENCE_VERSION=1.7.16`
`/apps/providence/app/conf/local/app.conf`: uncomment the line `# search_engine_plugin = ElasticSearch`
