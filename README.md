# lfd-bld-reporter [![Build Status](https://travis-ci.org/sjinks/lfd-bld-reporter.svg?branch=master)](https://travis-ci.org/sjinks/lfd-bld-reporter)

Blocklist.de Reporter for Login Failure Daemon (lfd)

## Description

lfd-bld-reporter is a script that integrates with ConfigServer's [Login Failure Daemon](http://configserver.com/cp/csf.html) and provides an ability to report the attacker and submit attack logs to [Blocklist.de](https://www.blocklist.de/en/index.html)

## Usage

First you will need to register an account with [Blocklist.de](https://www.blocklist.de/en/register.html). After that, you will need to [register your server](https://www.blocklist.de/en/profile/server.html), which will send reports to Blocklist.de.

Having that done, you will need to edit `/etc/lfd-bld-reporter/config.ini`:

```
[submitter]
apikey = APIKEY
server = EMAIL
```

where `APIKEY` is the automatically generated API key, which can be seen [here](https://www.blocklist.de/en/profile/server.html) (`API-KEY` column), `EMAIL` is the email address associated with the server (`email` column on the same page).

You will also need to have [CSF + LFD](http://configserver.com/cp/csf.html) installed (they have a good documentation and a well-commented configuration file).

Then, modify your CSF configureation file (`/etc/csf/csf.conf`): find the line

```
BLOCK_REPORT = ""
```

and replace it with

```
BLOCK_REPORT = "/usr/share/lfd-bld-reporter/lfd-bld-reporter.pl"
```

Save, then restart CSF and LFD:

```bash
csf -r
/etc/init.d/lfd restart
```
