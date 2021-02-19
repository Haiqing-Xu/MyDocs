# Steps for a Fresh Installation of MySQL

#### 1. Adding the MySQL APT Repository

 First, add the MySQL APT repository to your system's software repository list. Follow these steps:

a. Go to the download page for the MySQL APT repository at https://dev.mysql.com/downloads/repo/apt/.

b. Select and download the release package for your Linux distribution.

c. Install the downloaded release package with the following command, replacing version-specific-package-name with the name of the downloaded package

```bash
shell> sudo dpkg -i /PATH/version-specific-package-name.deb
```

d. During the installation of the package, you will be asked to choose the versions of the MySQL server and other components  that you want to install.

e. Update package information from the MySQL APT repository with the following command

```bash
shell> sudo apt update
```

#### 2. Installing MySQL with APT

```bash
shell> sudo apt-get install mysql-server
```

This installs the package for the MySQL server, as well as the packages for the client and for the database common files.

During the installation, you are asked to supply a password for the root user for your MySQL installation.

#### 3. Selecting a Major Release Version

By default, all installations and upgrades for your MySQL server and the other required components come from the release series of the major version you have selected during the installation of the configuration package (see Adding the MySQL APT Repository). However, you can switch to another supported major release series at any time by reconfiguring the configuration package you have installed. Use the following command:

```bash
shell> sudo dpkg-reconfigure mysql-apt-config
```

A dialogue box then asks you to choose the major release version you want. Make your selection and choose Ok. After returning to the command prompt, update package information from the MySQL APT repository with this command:

```bash
shell> sudo apt update
```

Install any packages of your choice with the following command, replacing package-name with name of the package (here is a list of available packages):

```bash
shell> sudo apt install package-name
```

For example, to install the MySQL Workbench:

```bash
shell> sudo apt install mysql-workbench-community
```

To install the shared client libraries:

```bash
shell> sudo apt install libmysqlclient21
```

