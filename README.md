# mongodb-logrotation


MongoDB log rotation work well when using Linux/Unix’s logrotate tool. Now I prefer this approach, since it doesn’t need the complex script writing that’s needed for the first log rotation method described above. Let’s see in detail how to configure log rotation with Linux/Unix’s logrotate utility.

For this log rotation automation, I used logrotate mechanism which are available in most of the linux distribution.

Mongdb configuration
In mongodb configuration file ( mongod.conf or mongos.conf ) at the systemLog section I provided below configuration. My log file path is /var/mongodb/log/mongod.log . After below changes, do not forget to restart the mongod or mongos process once again.

systemLog:
  
  destination: file
  
  path: /var/mongodb/log/mongod.log
  
  logAppend: true
  
  logRotate: reopen

Now create this file below, here mongodb is linux user name.

sudo nano /etc/logrotate.d/mongodb

In this file make this changes

/var/mongodb/log/mongod.log

{

rotate 10

daily

dateext

dateformat %Y-%m-%d-%s

dateyesterday

size 10000M

missingok

create 600 mongodb mongodb

delaycompress

compress

sharedscripts

postrotate

/bin/kill -SIGUSR1 $(cat /var/run/mongodb.pid)

endscript

}

pid file path for MongoDB

You will find this file to your data directory, in my case /data/db

/data/db/mongod.lock

Adding a simple file link will do the trick for me.

ln -s /data/db/mongod.lock /var/run/mongodb.pid

Now, this is linked to $(cat /var/run/mongodb.pid) of the logrotate setup file 

/etc/logrotate.d/mongodb which you are currently editing.










<img width="522" alt="Screenshot 2022-03-22 at 10 15 30" src="https://user-images.githubusercontent.com/75475233/159409900-377540fa-0c5e-49fa-b6a7-e3e95c7f4899.png">
