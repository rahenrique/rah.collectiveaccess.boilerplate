# CollectiveAccess Boilerplate

CollectiveAccess Boilerplate, using Docker containers, with PHP, Apache and MySQL.

This repository is intended as a starting point for a local environment for development of CollectiveAccess implementations.

**Work In Progress**. See the [issues page](https://github.com/rahenrique/rah.collectiveaccess.boilerplate/issues) for more information

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
