+++
title = "Parsing a Gunicorn access log in GoAccess"
date = "2021-03-23T15:00:00+02:00"
categories = ['blogposts']
draft = false
+++

I needed to analyze some logfiles which were produced by [Gunicorn](https://gunicorn.org) (a Python webserver).
<!--more-->
To enable the access-log, use the following option with Gunicorn (to log to STDOUT):

	gunicorn --access-logfile -

The default log format is:

	%(h)s %(l)s %(u)s %(t)s "%(r)s" %(s)s %(b)s "%(f)s" "%(a)s"

	this means:

	remote_address - user_name date "status_line" status response_length "referer" "user_agent"

example:

>63.35.111.242 - - [26/Jan/2021:06:25:16 +0100] "GET /api/v3/farms/3a9 HTTP/1.1" 301 0 "-" "python-requests/2.22.0"

For analyzing of webserver logfiles, [GoAccess](https://goaccess.io) can be used. A handy program which you can run right on your server.
# Parsing it
Our Gunicorn server runs in Docker and the output is logged to disk. If we open that logfile, it includes extra (Docker) stuff. You need to strip everything up to the first IP address. This is right after the string *'gunicorn.access:'*:

	sed 's/^.*gunicorn.access: //' app.log -i

After some fiddling I found the right format string. To parse it in GoAccess use the following line:

	goaccess --log-format='%h - %e [%d:%t] "%r" %s %b "%R" "%u"' --date-format=%d/%b/%Y --time-format='%T %z' -f app.log


NB: For a default Nginx access log, the COMMON format should work:

	goaccess --log-format=COMMON -f /var/log/nginx/access.log



# Resources:
- Gunicorn default log format: https://docs.gunicorn.org/en/latest/settings.html#access-log-format
- GoAccess custom log formats: https://goaccess.io/man#custom-log

