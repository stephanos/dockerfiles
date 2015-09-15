# PHP-56-FPM-POSTFIX

This image is the same as PHP-56-FPM, but instead of using SSMTP as email forwarder it starts
Postfix server.

It handles well situation when relay SMTP server is not accepting emails and is much faster then using SSMTP.
 
To configure Postfix as relay define environment variable XT_MAIL_SERVER. Use `POSTFIX_` prefixed
environemnt variables which will be applied with `postconf -e`.

If you want to execute script you have to:

* start this container with `docker run`
* `docker run` your script inside already running container
* `docker run` shutdown script to make sure that all emails were sent from queue

To make mail queue persistent you have to mount volume `/var/spool/postfix`.

##Start

    # start Docker container
    DID=$(docker run -d -e POSTFIX_relayhost=<my-smtp.example.com> \
      bigm/php-56-fpm-postfix)
      
    # excute your script
    docker exec -ti $DID php -f /xt/tools/mail_test.php <your email>
    
    # correctly shutdown the Docker instance to make sure that 
    docker exec -ti $DID /xt/tools/shutdown.sh
    