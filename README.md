# redis_explorer

To download the redis server and client.

This was done to get information from the Redis database from NPME.

NPME uses the couch and the redis database.

Redis is where the auth tokens for users are stored.

Database is installed by default in  

    # ls -l /usr/local/lib/npme/redis/
    total 396
    -rw-r--r-- 1 newrelic docker 399968 Jun  5 10:25 appendonly.aof


Installation of redis :

https://redis.io/topics/quickstart

For those impatient : Download and install this

    wget http://download.redis.io/redis-stable.tar.gz
    tar xvzf redis-stable.tar.gz
    cd redis-stable
    make
    make install

I installed this on both the Mac and Linux server and it works.

For exploring the redis db on NPME: Execute the redis-cli on command line
   If this fails - then check the IP address and the port .

    redis-cli

    Could not connect to Redis at 127.0.0.1:6379: Connection refused
    Could not connect to Redis at 127.0.0.1:6379: Connection refused
    not connected> exit


Resolution :
    
    root@dpek135:~# ps -ef |grep redis
    root      3481 30578  0 11:03 pts/0    00:00:00 grep --color=auto redis
    root     21302 21300  0 Apr13 ?        00:25:17 node bin/annotation-api.js start --redis-url=redis://172.17.0.1:6379
    root     21303 21298  0 Apr13 ?        00:00:01 node bin/npmo-auth-callbacks.js start --certificate= --redis=redis://172.17.0.1:6379 --entity-id=http://129.42.208.184:8082/auth/saml/metadata.xml --assert-endpoint=http://129.42.208.184:8082/auth/saml/assert --logout-endpoint=http://129.42.208.184:8082/auth/saml/logout --nameid-format=urn:oasis:names:tc:SAML:2.0:nameid-format:emailAddress --sso-login-url= --sso-logout-url=
    root     21463 21350  0 Apr13 ?        00:06:26 node ./bin/npm-auth-ws.js start --front-door-host=http://129.42.208.184:8080 --github-host=https://api.github.com --shared-fetch-secret=t_8gpjcx5I_DWJ9RUfLC7tpV0PAnfatpG7QN --authentication-method=fake --authorization-method=fake --session-handler=redis --reject-unauthorized=1 --port=5000 --host=0.0.0.0


Notice the port is IP is 172.17.0.1


So, start the 'redis-cli' with the IP as an argument. Notice the PONG response. This means the connection was successful.

    # redis-cli -h 172.17.0.1 -p 6379 ping
    PONG


To get the list of databases: Run the redis-cli command line as shown and type in CONFIG get databases.

    # redis-cli -h 172.17.0.1
    172.17.0.1:6379> config get databases
    1) "databases"
    2) "16"

To get the keys stored in redis
    keys *

    keys l*
