# check\_time\_http


A Nagios check to monitor the time offset between a web server (by examining its HTTP `Date` response header) and the local system (i.e. where the plugin is executed).


## Requirements

Nagios or Ininga, Python3 with the `argparse`, `sys`, `requests`, and `pytz` modules. The latter is `python2-tz` on Debian based systems.

## Usage
```
$ ./check_time_http --help
usage: check_time_http [-h] --url URL [--warn WARN] [--crit CRIT]
                       [--timeout TIMEOUT] [--verify | --no-verify]

Check time offset of the HTTP "Date" response header against the local time

optional arguments:
  -h, --help         show this help message and exit
  --url URL          the URL to check
  --warn WARN        Offset to result in warning (seconds, default 180)
  --crit CRIT        Offset to result in critical (seconds, default 300)
  --timeout TIMEOUT  Timeout of the HTTP request (seconds)
  --verify           Verify SSL certificate (default)
  --no-verify        Do not verify SSL certificate
```

## Notes
1. This plugin only looks at the HTTP Date header. Anything else is considered irrelevant and is not taken into account. So whether the actual service works fine (200), requires authentication (403), or even is failing (503) is not taken into account.

2. Since this plugin uses the difference between a remote system and the local system, it is recommended to make sure the local system time is accurately synced to an NTP source, and also monitor this, for instance with [`check_ntp_date`](https://www.monitoring-plugins.org/doc/man/check_ntp_time.html).

3. The format of the HTTP Date header limits the resolution to 1 second, so this plugin is only useful to pick up substantially larger offsets. The plugin was designed to give an early warning in case a web server that is performing a SAML role has its clock drifting away outside the recommended limits (3-5 minutes - which are the defaults for the plugin).

4. If there is no Date header detected in the response, the plugin returns UNKNOWN.