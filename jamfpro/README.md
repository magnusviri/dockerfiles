# Jamf Pro docker-compose

Docker Compose file to get [Jamf Pro](https://www.jamf.com/) running in a container. I have been using this to easily test [jctl](https://github.com/univ-of-utah-marriott-library-apple/jctl) against many different versions of Jamf Pro, which I run locally on my M1 laptop. I wanted to run my production server this way but alas, I never got up to the poing of running a production Docker server.

FYI: Jamf Pro is not free and is not included in this repository. You must purchase it before this will work. And even if you purchase Jamf, you will want to request a development license. Request the development license from your [Jamf Account Team](https://account.jamf.com/account-team) (login required).

Instructions for downloading Jamf Pro are below.

This docker-compose file mounts the Jamf Pro ROOT.war file. Docker saves a lot of time and disk space over creating a dedicated VM for running a development Jamf server. It also mounts the mysql data folder, so the database can saved even if the container is thrown away. You can keep around old versions this way too.

## Slack

If you have questions or want to discuss this please do it on the jamf-docker channel on the [MacAdmins Slack](https://www.macadmins.org/).

## Install

Download and install [Docker](https://www.docker.com/get-started) (docker compose is now part of docker).

### Clone Repository and Create Environment File

```
git clone github.com/magnusviri/dockerfiles.git
cd dockerfiles/jamfpro
cp .env_example .env
```

### Download Jamf Pro

Login to [https://account.jamf.com/products/jamf-pro](https://account.jamf.com/products/jamf-pro), find the version of Jamf Pro you want to download, select "Manual" then click "Download For Manual". Save your "Activation Code" since you will need to enter it once the server starts up.

Find the jamf-pro-installation-x.x.x.zip file you downloaded, and unzip it. Take the ROOT.war file that was unzipped and put it in the directory that represents the version. That is, if it's Jamf Pro 10.49.0, put it in a folder named 10.49.0 inside of this project. Like this.

```
> ls -lAR
total 64
-rw-r--r--@ 1 u0076374  staff   585 Aug 21 11:32 .env
-rw-r--r--@ 1 u0076374  staff   177 Aug 21 11:57 .env_example
-rw-r--r--@ 1 u0076374  staff    37 Mar 10  2022 .gitignore
drwxr-xr-x@ 3 u0076374  staff    96 Aug 21 12:16 10.49.0
-rw-r--r--@ 1 u0076374  staff  4529 Sep 11  2022 README.md
-rw-r--r--@ 1 u0076374  staff  1024 Aug 21 11:26 docker-compose.yml
-rwxr-xr-x@ 1 u0076374  staff  5227 Jul 27 15:30 wait-for-it.sh

./10.49.0:
total 755056
-rw-r--r--@ 1 u0076374  staff  386586348 Jul 24 10:34 ROOT.war
```

### Environment Variables

Edit the .env so that JAMF_PRO_VERSION is the correct version.

	JAMF_PRO_VERSION=10.49.0

Also, check the [Jamf Docker Hub page](https://hub.docker.com/r/jamf/jamfpro/tags) to see if they've updated their jamfpro tag. The latest version as of 2023-09-10 is 0.0.20. If they've updated it since then and you want to update your image, change this value to reflect the new tag.

	JAMF_IMAGE_VERSION=0.0.20.

If you want to change the MySQL values, do so as well. This repo specifies "latest."

Note, Jamf is using features that prevent it from working with MariaDB.

### Change the Port

This will open up port 80 on your computer. If you want it on a different port, open the docker-compose.yml file and change the "80" in the following lines to something else.

```
ports:
  - "80:8080"
```

### Start

```
docker compose up -d
```

Open [http://localhost/](http://localhost/) and wait for it to startup. It can take several minutes. Follow the setup procedures (here's the [documentation](https://www.jamf.com/resources/product-documentation/) in case you need help). Accept the license agreement, enter your activation code, and create an account. For the Jamf Pro URL use "http://localhost" (for local testing only) or whatever your IP is.

## Roadmap

As long as I am working on [jctl](https://github.com/univ-of-utah-marriott-library-apple/jctl) I will keep this repo updated and working.

I am not using this in production and don't plan to. Because of this, I am no longer planning on adding new features. If anyone would like to contribute or get this production ready, I would certainly update my repo with those changes.

## Credit

This depends completely on the image provided by [Jamf](https://github.com/jamf/jamfpro). I had nothing to do with developing that image.

I used [Bryson Tyrrell's file](https://gist.github.com/brysontyrrell/95c9492f02a691b3f976830557f6d4ed) as a starting point for this project.

I am also using [wait-for-it.sh](https://github.com/vishnubob/wait-for-it/blob/81b1373f17855a4dc21156cfe1694c31d7d1792e/wait-for-it.sh) to pause Jamf until MySQL starts.

## License

MIT License
