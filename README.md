[![Clojars Project](https://img.shields.io/clojars/v/org.neyho/eywa-core.svg)](https://clojars.org/org.neyho/eywa-core)
[![Clojars Project](https://img.shields.io/clojars/v/org.neyho/eywa-core-frontend.svg)](https://clojars.org/org.neyho/eywa-core-frontend)


#### EYWA Core
EYWA Core is consisted of IAM and Dataset modeling. Dataset modeling is heart of this project and
its goal is to enable users to model their data and deploy modeled data painlessly to underlaying DB
engine/service. With this approach data models are transportable (from DB to DB, from project to project),
reliable and provide common ground for extending application features.

Identity Access Management is necessary to enable secure and consistent way to control modeling, deployment,
and usage of deployed models so that only the right people/service/script can do the right thing. Therefore
IAM is indivisable part of Dataset modeling.


#### Installation
##### Windows

```ps1
Invoke-WebRequest -Uri "https://s3.eu-central-1.amazonaws.com/eywa.public/eywa_cli/install_eywa_cli.ps1" -OutFile eywa_cli_install.ps1
./eywa_cli_install.ps1
rm eywa_cli_install.ps1
```
Add ```%USERPROFILE%\.eywa\bin``` to PATH environment variable


##### Linux and MacOs
```bash
curl -s https://s3.eu-central-1.amazonaws.com/eywa.public/eywa_cli/install_eywa_cli.sh | bash
```


#### Quickstart
Currently supported database is Postgres, although support for MySQL, SQLite and others is on roadmap. 

Requirement is to have Postgres, so if you don't have installation i recommend using docker to download Postgres
image and afterwards run:
```
docker run --name EYWA_DEV -p 5432:5432 -e POSTGRES_PASSWORD=password postgres
```

This will spin up container with port forwarding so that you can connect to database on localhost using
postgres user.

Following steps are enough to initialize EYWA.

```
eywa core environment -s local
```
This will set environment to local, that will match docker container made above. Enviroments are
managed in configuration file that is on
```
eywa core config-path
```
To see what options are available to manage configuration file use:
```
eywa core config-schema
```
Lets get back on track, so we have running docker container and eywa cli application installed. As well
as local environment configured in configuration file and set as default environment for eywa cli.

Now we need to install EYWA Core jar file. Run
```
eywa core version -l

And output should look something like this
❯ eywa core version -l          
List of available versions. '*' installed, '-' not installed

[-] 0.1.4
[-] 0.1.3

```
To install some version run
```
eywa core version -s 0.1.4
```
Ok EYWA Core server is installed, now we need to initialize EYWA IAM and Datacraft. So we should run:
```
eywa core init
```
If above command didn't throw any error that implies that initialization were successfull and DB is initialized.
So everything is ready to start EYWA server, except there is no user that can login to EYWA.
```
eywa core super -s admin
```
Will prompt for password for admin password, and when supplied admin user will be created with target password.
Now run
```
eywa core start
```
And navigate to **http://localhost:8080/eywa/** and login screen should be waiting for you. Use username and
password from previous step to login.

Also there is GraphiQL ui available at http://localhost:8080/graphql/ui . To access it you need to login to EYWA
ui first.


To track what is happening open log file at location __~/.eywa/logs/dev.log__


If something went wrong or EYWA Core server isn't running as supposed to, run
```
eywa core doctor
```



#### Development setup
Check out [setup.md](./doc/setup.md)


#### Dataset sources
For working on datasets and testing reconsider following datasets:
 * https://musicbrainz.org/
 * https://developer.imdb.com/non-commercial-datasets/
 * https://archive.org/details/stackexchange
 * Python - [Faker](https://faker.readthedocs.io/en/master/)
