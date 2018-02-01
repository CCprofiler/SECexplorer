SECexplorer
===========

SECexplorer is a web interface to the *R* package *CCprofiler*.

This project consists of two parts:

- The `client/` folder contains a javascript-based frontend making use of the framework
  [angularjs](https://angularjs.org/).

- A `server/` folder holds Python code for the backend server based on
  [flask](http://flask.pocoo.org/).  Further the backend uses `rpy2` to execute the functions from the
  `SECprofiler` R package.


How is the web server setup ?
-----------------------------

When we open a web browser and request `http://sec-explorer.ethz.ch` we communicate with
a `nodejs` web server listening on port 80. This server is configured and setup in the `client` folder.

Currently this web server only serves the `index.html` file plus the referenced static java script and css files. 

The clients  javascript code requests the functions from the `R` pacakge `CCprofiler` by accessing the backend server who offers a web service on the URL `http://127.0.0.1:8020/app`. 

This backend Python server is implemented with `flask`. Apache listens on port 8020 and forwards requests to the `app.wsgi` script using the apache module `mod_wsgi`.


How to setup and install the web server
----------------------------------------

**First read the `client/README.md` file for installing `nodejs` and the requires javascript 
packages.**

**Furter check the installation instructions in the `CCprofiler` package. This `R` package must be
accesssible by `rpy2` within the Python backend.**

We implemented a unit file `sec_explorer.server` for `systemd` for proper startup and shutdown of
this processes. More details below.


*The following instructions where developed and tested on `Ubuntu 16.04.1 LTS`. They should work for
new versions as well, but might break on older versions as `systemd` was not default system manager
before `Ubuntu 15.04`.*

To install the `sec_explorer.service` file and to configure apache  we assume that your current
working directory is the one holding this `README.md` file. Then run:

```bash
$ sudo ln -s $(pwd)/sec_explorer.service /lib/systemd/system
$ sudo ln -s $(pwd)/for_apache.conf /etc/apache2/conf-enabled/sec-explorer.conf
$ sudo systemctl enable sec_explorer.service
```



Development on the web server setup
==================================

In case you edit the `sec_explorer.service` file you first have to reload it:
```bash
$ sudo systemctl daemon-reload
```


How to start / stop the web server
=========================

```bash
$ sudo systemctl start sec_explorer.service
```

If this fails or in case you need more information about the status of the involved processes run

```bash
$ sudo systemctl status sec_explorer.service apache2.service
```

```bash
$ sudo systemctl stop sec_explorer.service
```
