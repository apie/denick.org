+++
title = "How to log POST data with Nginx"
date = "2024-05-18T17:00:00+02:00"
categories = ['blogposts']
tags = ['howto', 'nginx']
+++

A customer was getting an error message, but he could not tell me exactly what his program was sending and we did not properly log the error. The quickest solution was to add extra logging in Nginx, so no changes to our running app had to be made.
<!--more-->

To do this,
- Define a log format in Nginx. We shall call it 'postdata' and it only logs POST data.
- Add a location block of the endpoint you want to log (since we do not want/need to log all the endpoints).
- Specify a location to log this endpoint to. Specify our custom log format.
- Do not forget to duplicate your normal handling in this location block, otherwise the POST gets logged, but not handled.

It looks like this in the nginx config file:
```
log_format postdata $request_body;
server {
    location /post_endpoint {
        access_log /tmp/nginx-access-post.log postdata;
        # your normal handling, proxy_pass for example
    }
    location / {
        # ...
    }
}
```

The next day, the customer's script ran again and the data was logged to the file. We found the bug directly and could remove the logging.
