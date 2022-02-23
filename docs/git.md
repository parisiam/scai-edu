<h1>INSTALLATION AND UPDATE USING GIT</h1>

[TOC]

# In a nutshell

- Git is very usefull when installing and upgrading the **core Moodle program**: It makes things much simpler.
- However, to install or upgrade the additional **plugins**, even when using git for the core program, it it still possible (and simpler) to use Moodle web interface.
- When using git, the additional plugins MUST be excluded from git using the **.git/info/exclude** file and NOT the .gitignore file.

# Moodle documentation

► Moodle installation using Git: https://docs.moodle.org/311/en/Git_for_Administrators (3x/fr for the French version)
► Standard Moodle installation: https://docs.moodle.org/311/en/Installing_Moodle
► Upgrading Moodle: https://docs.moodle.org/311/en/Upgrading
► Information about the types of plugins: https://docs.moodle.org/dev/Plugin_types

# Installing Moodle

These steps are valid for the creation of a new installation or when updating an existing site. When you are updating the Moodle, before any operation, don't forget to backup :

- The **database**.
- The **moodledata** folder
- The **config.php** file
- The **addtional plugins** (themes, mods, filters...)

## Create the Moodle repo

The directory structure is:

```text
/path/to/moodle
├── moodledata/  <-- this directory must NOT be seen from internet
└── public/ 	 <-- website url points here
```

- If you <u>create</u> a new Moodle site:

```sh
$ cd /path/to/moodle # A /public folder may exist inside webroot, but it MUST be empty
```

Then this:

```sh
$ git clone git://git.moodle.org/moodle.git public # Downloads ALL versions of Moodle ~500Mo (very long)
$ cd public
$ git branch -a # Check all available branches
$ git branch --track MOODLE_311_STABLE origin/MOODLE_311_STABLE # --track: default branch for pull
$ git checkout MOODLE_311_STABLE
```

Or this:

```sh
$ git clone -b MOODLE_311_STABLE git://git.moodle.org/moodle.git public 
# Downloads also ALL versions of Moodle ~500Mo (but no local master branch, which is good)
```

*Cloning through git may take a minute or 2. It should get the latest stable version of Moodle (including latest fixes).*

- If you <u>update</u> an existing Moodle site, rename the directory containing the existing Moodle program and proceed as above.

```sh
$ cd /path/to/moodle
$ mv public public_ # for example
# Then proceed as above
```

From here the Moodle program is installed BUT the **database**, the **/moodledata** folder and **config.php** file are missing. The **modules** need a special treatment too.

## Create the moodledata folder

- If you <u>create</u> a new Moodle site, create a new **moodledata** folder.

```sh
$ cd /path/to/moodle
$ mkdir moodledata # Make sure the directory is writable
```

- If you <u>update</u> an existing Moodle site, reuse the **moodledata** folder.

```sh
$ cd /path/to/moodle
$ mkdir moodledata # OR below
$ cp -rp /path/to/old/moodledata moodledata # -p to preserve owner, dates, permissions
# OR copy the moodledata.tar.gz file and unzip it
```

OR copy the moodledata.tar.gz file in the moodle folder and unzip it

```sh
$ cd /path/to/moodle
$ tar -xzf moodledata.tar.gz
```

### Permissions on moodledata folder

The moodledata directory must be **readable AND writeable** by the web server user which depends on your system. 

```sh
$ chown -R <www_user>:<www_user> /path/to/moodle/moodledata
$ cmod -R 0750 /path/to/moodle/moodledata # Adjust permissions according to your server
```

> The user might be the webserver user or the root user, it depends on the server. www-data or root, for example.

Note: you might need access to sudo to change permissions and ownership.

Make sure this is coherent to what's in the `config.php` file:

```php
$CFG->directorypermissions = 0777; # Maybe avoid the default 0777 (it might be unsafe)
```

> Usually the permission is 0750, 0755 or 0775, it depends on the server.

## Create the database

- If you are <u>create</u> a new Moodle site, create a new database.
- If you <u>update</u> an existing Moodle site, reuse the **database** (don't forget to make backups).

Collation: **utf8mb4_unicode_ci**.

## Setup  the config.php file

- If you <u>create</u> a new Moodle site, create a **config.php** file in root folder and set it up:

```php
<?php
unset($CFG);
global $CFG;
$CFG = new stdClass();
$CFG->dbtype    = 'mysqli';
$CFG->dblibrary = 'native';
$CFG->dbhost    = 'localhost';
$CFG->dbname    = 'my_dbname'; // To adjust
$CFG->dbuser    = 'my_dbuser'; // To adjust
$CFG->dbpass    = 'my_dbpass'; // To adjust
$CFG->prefix    = 'mdl_';
$CFG->dboptions = array (
  'dbpersist' => 0,
  'dbport' => '',
  'dbsocket' => '',
  'dbcollation' => 'utf8mb4_unicode_ci',
);
$CFG->wwwroot   = 'https://my_moodle_url';  // USE https NOT http, NO trailing /
$CFG->dataroot  = '/path/to/moodledata'; 
$CFG->admin     = 'admin';
$CFG->directorypermissions = 0777;
require_once(__DIR__ . '/lib/setup.php');
// Upgrade key is used when the site is being upgraded though the web interface.
// It should not be the case, as we are upgrading now through git. But it prevents 
// anonymous access to the upgrade screens, and accidental upgrade through the website.
$CFG->upgradekey = 'secret';
```

- If you update an existing Moodle site, reuse the **config.php** (don't forget to adjust it, if necessary, and to back it up).

## Empty the caches

If you update from an existing Moodle, better empty the cache before accessing the new installation. An easy way to empty the cache is to empty the following sub-directories from moodledata (and **ONLY** these ones):

```text
moodledata/
├── cache/      <-- Empty this folder
├── localcache/ <-- Empty this folder
└── sessions/   <-- Empty this folder (it will delog all users)
*** DO NOT EMPTY ANY OTHER FOLDER (do not mess around, believe me) ***
```

```sh
$ cd /path/to/moodle
$ rm -rf moodledata/cache/{*,.*}
$ rm -rf moodledata/localcache/{*,.*}
$ rm -rf moodledata/sessions/{*,.*}
```

## Add the plugins

- When creating a new Moodle installation, there are no additional modules, so there is nothing to do, for now.

- When you update and existing Moodle, you must identify all the modules you have added.
  Copy all the modules installed in the old version: Each module is normally located into a directory indicated below.

### Plugins used

The list of installed plugins are found through *Site administration > Plugins > Plugins overview*, and then *All plugins*.
The plugins marked as **Additional** in the list are those installed on top of the standard Moodle installation.

I've seen 2 diferent screens listing the plugins. On one of them, the path where the plugin is found is indicated : `https://<moodle_url>/admin/index.php?cache=0&confirmrelease=1&confirmplugincheck=0&showallplugins=1`

```text
theme/moove
course/format/tiles
filter/filtercodes
local/boostnavigation
mod/coursecertificate
admin/tool/certificate
filter/sagecell
```

```sh
# Tar files in current Moodle
cd /path/to/old/moodle
mkdir plugins
tar -czf plugins/moove.tgz public/theme/moove
tar -czf plugins/tiles.tgz public/course/format/tiles
tar -czf plugins/filtercodes.tgz public/filter/filtercodes
tar -czf plugins/boostnavigation.tgz public/local/boostnavigation
tar -czf plugins/coursecertificate.tgz public/mod/coursecertificate
tar -czf plugins/certificate.tgz public/admin/tool/certificate
tar -czf plugins/sagecell.tgz public/filter/sagecell
# Untar files in new installation (files already copied in plugins folder in new installation)
# No need to specify the path (as in: -C public/theme): it was registered when taring
cd /path/to/new/moodle
tar -xzf plugins/moove.tgz
tar -xzf plugins/tiles.tgz
tar -xzf plugins/filtercodes.tgz
tar -xzf plugins/boostnavigation.tgz
tar -xzf plugins/coursecertificate.tgz
tar -xzf plugins/certificate.tgz
tar -xzf plugins/sagecell.tgz
```

## When adding a plugin

Any additional plugin should be excluded from git, but NOT using the `.gitignore` file which should remain untouched (it will be updated with the rest of the files next time you use git to update Moodle).

Instead, it should be manually added to the **.git/info/exclude** file:

```sh
$ cd /path/to/moodle
$ echo /path/to/plugin/ >> .git/info/exclude
```

```sh
cd /path/to/moodle # Moodle root, NOT public/
echo theme/moove/ >> public/.git/info/exclude
echo course/format/tiles/ >> public/.git/info/exclude
echo filter/filtercodes/ >> public/.git/info/exclude
echo local/boostnavigation/ >> public/.git/info/exclude
echo mod/coursecertificate/ >> public/.git/info/exclude
echo admin/tool/certificate/ >> public/.git/info/exclude
echo filter/sagecell/ >> public/.git/info/exclude
```

## Cron

Setting up the cron is **ESSENTIAL** and it should run **every minute**.

```sh
# Open the cron file and update the file
$ crontab -e
```

```php
# Adjust the paths and variables
# MAILTO="my@email.com"
# SHELL="/bin/bash"
*/1 * * * * /usr/local/bin/php /path/to/moodle/public/admin/cli/cron.php
```

You may also run manually the CRON, if allowed in advance by the administrator and if a password was set:

```html
https://<moodle_url>/admin/cron.php?password=<password>
```

## Server: SSL certificate, PHP ini

Make sure the server is ready:

- SSL certificate
- PHP ini (see below)
- Web root
- ...

## Email settings

It's complicated. We don't explain that here.

## Light the fire

We're almost there. You must open the admin url and finish the intallation in the browser:

```
https://<url_to_moodle>/admin
```

Follow the instructions on screen and adjust the server of necessary.

Mostly click on the *Update the database* button for the first round, without updating any plugin.

If you are sent to the login screen, it means the process was successful. If not, you might be caught in the "update loop of hell" (you can't leave the update process). See Problems section below.

At that point Moodle has been upgraded. To make sure of it, sign in as admin and go to *Site administration > Notifications:

- Check the Moodle version at the bottom of the page.
- Click the **plugin overview** link.
- Upgrade the plugins by clicking the *Install this update* buttons, if any. 

# Upgrading the git version of Moodle

► https://docs.moodle.org/311/en/Upgrading

- To check all the versions available on the remote:

```sh
$ git remote show origin
```

- To check if the remote has been updated and if we are behind:

```sh
$ git status -uno 
# or:
$ git fetch # git fetch origin ?
$ git status
```

- If there is a new version (there should be weekly updates), just **pull** the content:

```sh
$ git pull
```

> Of course you should make a copy and test the upgrade on the copy before upgrading the production version (and make backups, and delog the users).

# Upgrade scenario

**<u>First steps</u>**:

- Put server in maintenance mode : Go to `Adminsitration > Server > Maintenance mode`.
- Zip the `public/` and the `moodledata/` folders, backup the `config.php` file.
- Make a sql dump of the database.

**<u>Install and test the upgrade a on copy of the website</u>**:

- Install a copy of the Moodle on a different location or a different server (local server for example).
- Upgrade the copy and empty the caches (see above).
- Access the home page of the Moodle to finalize the process and upgrade the plugins if required.
- Log in as admin at `http://<moodle_url>/admin`.It should take you to the upgrade screens.
- Once the upgrade process is finished, go to `Administration > Notifications` to make sure everything is up to date.
- Check that everything else is ok: from the admin account, log in as a student, access courses...

if something goes wrong, start again the upgrade process on the copy.

**<u>When everything is ok with the upgraded copy</u>**:

- Replace the production website by the upgraded copy.
- Open the home page and check it is ok.
- Log in as admin and go to *Administration > Notifications* to make sure everything is up to date.
- Check that everything else is ok: log in as a student, access courses... from the admin user.

**<u>Back to normal</u>**:

- Remove the maintenance mode with an admin account at https://moodle.parisiam.com/login/index.php

# Problems encountered when upgrading

Upgrading a Moodle is not a smooth operation and many problems may occur.

## Importing the database fails

- The sql dump is too big: try to **zip** it or **tar.gz** it.

- The import of a zipped file fails. Try different ways of zipping it.

  - Try to zip it (using the finder or an app) and import it with PHPMyAdmin.
  - Try to tar.gz it (using path finder or if it still fails, using command line) and import it with PHPMyAdmin.
  - Try to import it unzipped from the command line (if you have access to mysql in command line)

  *For me, the zip and PhpMyAdmin worked on my server and command line worked on my local machine.*

## Images are broken

![image-20220223102944019](.img/git/image-20220223102944019.png)

- Empty the caches folders (cache, localcache, sessions).
- Check and adjust the permissions of the moodledata folder.
- Reinstall the moodledata folder from the backup (if it's not an empty moodle you are installing).

## Update loop of hell

Coming soon.

# Command lines for MySQL

Here is a series of MySQL commands to prepare the database directly from the command line. It's not possible on every host.

```sh
# Start mysql (you may create and use an alias)
$ /path/to/mysql --host=<host> -u<user> -p<password>
```

```mysql
# Query the database, the charset and the collation (non destructive)
SHOW VARIABLES LIKE  'char%'; # Default charset on the system
SHOW DATABASES; # List of databases
SHOW DATABASES LIKE 'moodle_%'; # Filtered list of databases
SELECT * FROM INFORMATION_SCHEMA.SCHEMATA; # Charsets and collations
SELECT * FROM INFORMATION_SCHEMA.SCHEMATA WHERE SCHEMA_NAME LIKE 'moodle_%';
SELECT DATABASE(); # Show current database (if any)
```

```mysql
# Create a new database
CREATE DATABASE db_name;
CREATE DATABASE db_name CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
# Delete a database
DROP DATABASE db_name; # Just in case (attention, no warning)
# Open the database
USE db_name;
# Import an SQL dump
SOURCE path/to/file.sql;
# The part to add a user to the database is not presented here (yet)
```

```mysql
# It might be necessary to create a mysql user and/or grant permission on the database
# Here are a few useful commands
SELECT host, user FROM mysql.user; # List all users and host
SHOW GRANTS FOR 'username'@'localhost'; # Show permissions of a user
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON db_name.* TO 'username'@'localhost'; # Set permissions
FLUSH PRIVILEGES; # Reloads the privileges
```

```sh
# Make an sql dump and remove definer (you may create and use an alias for mysqldump)
$ path/to/mysqldump --host=<host> -u<user> -p<password> db_name --routines --set-charset --skip-add-locks --skip-extended-insert | sed -e 's/DEFINER=[^ ]* / /' | sed -e 's/DEFINER=[^*]*\*/\*/' > path/to/dump.sql
```

