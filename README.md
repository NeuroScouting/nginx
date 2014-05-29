nginx
=====

Nginx source from ubuntu ppa with push stream module enabled. 
See http://www.nginx.org and https://github.com/wandenberg/nginx-push-stream-module#installation

To build:

```shell
git clone git@github.com:NeuroScouting/nginx.git
cd nginx
git submodule init
git submodule update
```

Make sure your machine is ready to build:

```shell
# need this for extra dependencies. only do the first time.
sudo add-apt-repository ppa:nginx/stable
sudo apt-get update
sudo apt-get install dpkg-dev
sudo apt-get build-dep nginx
# it looks like the build-deps are not 100% correct and you may 
# have to install this package too:
sudo apt-get install libgd2-noxpm-dev
```

Now build nginx:

```shell
cd nginx-1.6.0
# -b to avoid building the source package
fakeroot dpkg-buildpackage -uc -us -b
```

Once it's done, the deb packages will be inside of nginx, you can install them:

```shell
cd ..
sudo dpkg -i nginx-light*deb nginx-common*deb
sudo apt-get -f install
```

and verify everything is kosher:
```shell
nginx -V 2>&1 | grep -i --color push-stream
```

and verify that nginx likes push stream directives:
```shell
# assuming you're in the folder where you unpacked the nginx source
mkdir logs
nginx -p `pwd` -c nginx-1.6.0/debian/modules/nginx-push-stream-module/misc/nginx.conf -t
```

output like this means you're good to go:
```
nginx: [alert] could not open error log file: open() "/var/log/nginx/error.log" failed (13: Permission denied)
2014/05/29 15:33:04 [info] 25354#0: Using 102400KiB of shared memory for push stream module on zone: push_stream_module in /tmp/nginx/nginx-1.6.0/debian/modules/nginx-push-stream-module/misc/nginx.conf:21
nginx: the configuration file /tmp/nginx/nginx-1.6.0/debian/modules/nginx-push-stream-module/misc/nginx.conf syntax is ok
nginx: configuration file /tmp/nginx/nginx-1.6.0/debian/modules/nginx-push-stream-module/misc/nginx.conf test is successful
```
