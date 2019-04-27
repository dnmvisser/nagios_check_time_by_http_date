# check\_time\_http


A Nagios check to monitor the time offset between a web server (by examining its HTTP `Date` response header) and the local system (i.e. where the plugin is executed).


## Requirements

Nagios or Ininga, Python3 with the `argparse`, `sys`, `requests`, and `pytz` modules. The latter is `python2-tz` on Debian based systems.

## Usage
```
$ ./check_time_http --help
usage: check_time_http [-h] --url URL [--warn WARN] [--crit CRIT]

Check time offset from HTTP "Date" header against local time

optional arguments:
  -h, --help   show this help message and exit
  --url URL    the URL to check
  --warn WARN  Offset to result in warning (seconds, default 180)
  --crit CRIT  Offset to result in critical (seconds, default 300)
```

## Notes
1. This check only looks at the HTTP `Date` repsonse header. Anything else is considered irrelevant and is not taken into account. So whether the actual service works fine (200), requires authentication (403), or even is failing (503) is not taken into account.


2. Since this plugin uses the difference between a remote system and the local system, it is recommended to make sure the local system time is accurately synced to an NTP source, and also monitor this, for instance with [`check_ntp_date`](https://www.monitoring-plugins.org/doc/man/check_ntp_time.html).