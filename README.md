BitCurator Access Webtools: Disk Image Access for the Web
------------------------------------
The bca-webtools project provides simple access to disk images over the web using open source 
software including The Sleuth Kit, PyTSK, the Flask web framework, and postgres.

Simply point bca-webtools at a local directory that contains raw (dd) or forensically-packaged disk 
images, and it will create a web portal that allows you to browse the file systems, download 
files, and examine disk image metadata.

Find out more at <http://access.bitcurator.net/>

See a previous version of bca-webtools (DIMAC) in action at <http://www.youtube.com/watch?v=BwiWFqxYzQ8>.

# Getting started
This software uses Vagrant to provision a virtual machine in which bca-webtools runs. To start, make sure you have VirtualBox installed:

  * http://www.virtualbox.org/

and Vagrant installed:

  * https://vagrantup.com

To check out the bca-webtools code repo, run:

  * git clone https://github.com/kamwoods/bca-webtools

Once you have this repository cloned out, simply run "vagrant up" from within the bca-webtools directory. There's a sample image in the "disk-images" directory to get you started. Once the virtual machine has been provisioned, open a web browser on your host and navigate to:

  * 127.0.0.1:8080

to see the flask application running.

# Dependencies

The dependencies listed below are automatically downloaded and installed (or compiled) when Vagrant provisions the virtual machine. They are included here for reference.

The bca-webtools project is a Flask application. It has been tested with Python 2.7.3, Flask 0.11, Jinja2, and Postgres 9.3 (but will likely work with other versions). Python 3 should also work..
You'll also need a range of other forensics tools, including AFFLIB (v3.7.4 or later), libewf (20140427 or later), The Sleuth Kit (4.1.3 or later), and PyTSK.

On a Debian or Ubuntu system, fulfilling the some of these dependencies is easy. Others are a bit more involved, as the required versions are not packaged. The instructions below should help, but keep in mind - this is an early version of the tool. If you run into trouble, post a message to the Github repo. 

Install python-pip and Flask:
-----------------------------

  * sudo apt-get install python-pip
  * sudo pip install flask

In order to use the database backend, you'll also need postgresql. On a Ubuntu or Debian system, you can install this with the following command:

  * sudo apt-get install postgresql

PGAdmin 3 will simplify the process of managing databases you may use with the application. You can install this with:

  * sudo apt-get install pgadmin3

In order to build server-side applications (and configure psycopg2 for use by the Flask app) you'll need the postgresql server development package:

  * sudo apt-get install postgresql-server-dev-9.3

Install psycopg2 and SQLAlchemy (required):
-------------------------------------------

  * sudo pip install -U psycopg2
  * sudo pip install Flask-SQLAlchemy

Install libtalloc (required):
-----------------------------
  * sudo apt-get install libtalloc2
  * sudo apt-get install libtalloc-dev

Set up the database:
--------------------

We need to change the PostgreSQL postgres user password; we will not be able to access the server otherwise. As the “postgres” Linux user, execute the psql command. In a terminal, type: 

  * sudo -u postgres psql postgres

Now, in the postgres prompt, type the following:

  * \password postgres

You’ll need to enter a password, twice, for the “postgres” (master RDBMS control) user. You won’t see the password appear as you type it (twice). Our example below assumes you use the master user "bcadmin".

Now hit “Ctrl-D” to quit the pgsql prompt.

Now, it’s time to create a new user who can create new databases but not other users (note that you won’t see the characters you type in as the password):

  * sudo -u postgres createuser -A -P bcadmin
  * Enter password for new role: bcadmin
  * Enter it again: bcadmin
  * Shall the new role be allowed to create databases? (y/n) y
  * Shall the new role be allowed to create more new roles? (y/n) n

Now, create a new database (in this example, we'll call it “bcdb”) with user access rights for the user we created earlier (the "bcadmin" user, in this example, and the -O in the following line is a capital letter O, not a zero): 

  * sudo -u postgres createdb -O bcadmin bcdb

Finally, do a full restart on the database, and you should be ready to go:

  * sudo /etc/init.d/postgresql restart

Install libewf:
---------------

Download the current libewf code from the downloads link at https://code.google.com/p/libewf/. Unpack the .tar.gz file, change into the libewf directory, and run the following:

  * ./bootstrap
  * ./configure --enable-v1-api
  * make
  * sudo make install
  * sudo ldconfig

Install The Sleuth Kit:
-----------------------

Download the current master source from http://www.sleuthkit.org/sleuthkit/download.php. 

Note 1: There's an older version of The Sleuth Kit available as a package in Ubuntu 14.04LTS - don't use it.

Note 2: As of TSK 4.1.3, you will need to apply a minor patch in order for pytsk to build in a later step. You can find this patch at:

  * https://4a014e8976bcea5c2cd7bfa3cac120c3dd10a2f1.googledrive.com/host/0B3fBvzttpiiScUxsUm54cG02RDA/tsk4.1.3_external_type.patch

Unpack the .tar.gz file, change into the sleuthkit directory, and run the following:

  * ./configure
  * make
  * sudo make install
  * sudo ldconfig

Install The Sleuth Kit Python bindings:
---------------------------------------

Download the current pytsk (TSK Python bindings):

  * git clone https://github.com/py4n6/pytsk

Change into the pytsk directory, and run the following:

  * python setup.py build
  * sudo python setup.py install

# bca-webtools Documentation

More documentation coming soon. bca-webtools is currently in alpha; updates will be posted here and on our website at [http://access.bitcurator.net/](http://access.bitcurator.net/) as they become available.

# License(s)

bca-webtools project documentation, and other non-software products of the BitCurator Access team are subject to the the Creative Commons Attribution 3.0 Unported license (CC BY 3.0).

Unless otherwise indicated, software objects in this repository are distributed under the terms of the GNU General Public License, Version 3. See the text file "COPYING" for further details about the terms of this license.


