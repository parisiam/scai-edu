<h1>INSTALLATION AND UPDATE USING GIT</h1>

[TOC]

# In a nutshell

- Git is very usefull when installing and upgrading the **core Moodle program**: It makes things much simpler.
- However, to install or upgrade the additional **plugins**, even when using git for the core program, it it still possible (and simpler) to use Moodle web interface.
- When using git, the additional plugins MUST be excluded from git using the **.git/info/exclude** file and NOT the .gitignore file.

# References

► Moodle installation using Git: https://docs.moodle.org/311/en/Git_for_Administrators (3x/fr for the French version)

► Standard Moodle installation: https://docs.moodle.org/311/en/Installing_Moodle

► Information about the types of plugins: https://docs.moodle.org/dev/Plugin_types

# Installing Moodle from an existing site or not

These steps are valid for the creation of a new installation when updating an existing site. When you are updating the Moodle, before any operation, don't forget to backup :

- The **database**.
- The **moodledata** folder
- The **config.php** file
- The **addtional plugins** (themes, mods, filters...)

## Create the Moodle repo

The directory structure is:

```text
/path/to/your/webroot/
├── moodledata/  <-- this directory must NOT be seen from internet
└── public/ 	 <-- website url points here
```

- If you <u>create</u> a new Moodle site:

```sh
$ cd /path/to/your/webroot # A /public folder may exist inside webroot, but it MUST be empty
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
$ cd /path/to/your/webroot
$ mv public public_ # for example
# Then proceed as above
```

From here the Moodle program is installed BUT the **database**, the **/moodledata** folder and **config.php** file are missing. The **modules** need a special treatment too.

## Create the moodledata folder

- If you <u>create</u> a new Moodle site, create a new **moodledata** folder.

```sh
$ cd /path/to/your/webroot
$ mkdir moodledata # Make sure the directory is writable
```

- If you <u>update</u> an existing Moodle site, reuse the **moodledata** folder.

```sh
$ cd /path/to/your/webroot
$ mkdir moodledata # OR below
$ cp -rp /path/to/old/moodledata moodledata # -p to preserve owner, dates, permissions
```

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
/theme/moove/
/course/format/tiles/
/filter/filtercodes/
/local/boostnavigation/
/mod/coursecertificate/
/admin/tool/certificate/

=====> currently installed but not needed
/filter/sagecell/

=====> to remove
/local/profilecohort/
/local/staticpage/
```

## When adding a new plugin

Any new module added (whatever the means), should be excluded from git, but NOT using the `.gitignore` file which should remain untouched (it will be updated with the rest of the files next time you use git to update Moodle).

Instead, it should be manually added to the **.git/info/exclude** file:

```sh
$ cd /path/to/your/moodle/
$ echo /path/to/plugin/ >> .git/info/exclude
```

## Cron

Setting up the cron is **ESSENTIAL** and it should run **every minute**.

```php
/usr/local/bin/php /path/to/moodle/admin/cli/cron.php
```

# Updating the git version

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

