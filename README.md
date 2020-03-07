# Instructions

[ScotchboxPro](https://box.scotch.io/) gives you a super simple vagrant LAMP stack for local development.

To get it: `git clone https://github.com/scotch-io/scotch-box-pro ` followed by a directory name.

The server will be at `192.168.33.10`

## Fixing the 404 errors on vagrant up

Unfortunately, scotchbox uses an Ubuntu release that’s not supported (and scotchbox itself doesn’t seem to be supported anymore) so when you run `vagrant up` for the first time, you’ll get a load of 404 errors. Here’s how to fix that:

* `vagrant ssh`
* `sudo sed -i -e 's/archive.ubuntu.com\|security.ubuntu.com/old-releases.ubuntu.com/g' /etc/apt/sources.list`
* `sudo apt-get update`
* `exit`
* `vagrant halt`
* `vagrant up`

## Running from root

If you clone scotchbox pro, by default it sets the document root to be /public. This is a little inconvenient but easily fixed.

* `cd` to the project directory
* Get Scotchbox Pro and put it into a temp directory `git clone https://github.com/scotch-io/scotch-box-pro tmp`
* Move the Vagrantfile out of tmp into root. Delete tmp
* `vagrant up`
* if lots of 404 errors - see above

* `vagrant ssh`
* `sudo vim /etc/apache2/sites-enabled/000-default.conf`

**Using Craft? STOP** - see the additional instructions below.

* remove `/public` in two places so the docuement root is just `/www/` (`i`)
* exit vim editor (esc key), then `:wq`
* restart apache `sudo apachectl restart`

## Database connections
* user: `root`
* pass: `root`
* database: `scotchbox`
* host: `localhost`

## Additional instructions for Perch:
* grab a copy of the database (there’ll be one in /dev) and import it to the scotchbox database
* make sure the perch config show the correct database connection details (as above)

## Additional instructions for Craft

You can’t run from root for craft, instead it wants you to run from `/web` so follow the instructions from **Running from root** but instead of removing `/public` from the document root. Change `public` to `web` (in two places). Then restart.

* `vagrant ssh` (if not already there)
* navigate to `/var/www/` `composer install`

* get a copy of the database and import it into scotchbox
* create a .env file in the project directory and change the values (as above)

## Thanks
* This issue solves the 404: https://github.com/scotch-io/scotch-box-pro/issues/23
* This video was useful in changing document root & general craft setup: https://craftquest.io/courses/localhosting-craft-cms/4312
