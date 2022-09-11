# Jamf Pro docker-compose

Docker Compose file to get [Jamf Pro](https://www.jamf.com/) running in a container. I have been using this to easily test [jctl](https://github.com/univ-of-utah-marriott-library-apple/jctl) against many different versions of Jamf Pro, which I run locally on my M1 laptop. I plan on eventually running my production server this way.

FYI: Jamf Pro is not free and is not included in this repository. You must purchase it before this will work. And even if you purchase Jamf, you will want to request a development license. Request the development license from your [Jamf Account Team](https://account.jamf.com/account-team) (login required).

Instructions for downloading Jamf Pro are below.

This docker-compose file mounts the Jamf Pro ROOT.war file. This saves a lot of disk space and time. It also mounts the mysql data folder, so it can saved even if the container is thrown away.

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

Find the jamf-pro-installation-x.x.x.zip file you downloaded, and unzip it. Take the ROOT.war file that was unzipped and put it in the directory that represents the version. That is, if it's Jamf Pro 10.26.1, put it in a folder named 10.26.1 inside of this project. Like this.

```
ls -lR
total 32
drwxr-xr-x  4 james  staff   128 Jan 22 13:11 10.26.1
-rw-r--r--  1 james  staff   153 Jan 22 13:11 Dockerfile
-rw-r--r--  1 james  staff  1071 Jan 22 12:49 LICENSE
-rw-r--r--  1 james  staff  1598 Jan 22 13:17 README.md
-rw-r--r--  1 james  staff   768 Jan 22 13:11 docker-compose.yml

./10.26.1:
total 557312
-rw-r--r--  1 james  staff  285159621 Dec  2 17:42 ROOT.war
```

### Environment Variables

Edit the .env so that JAMF_PRO_VERSION is the correct version.

	JAMF_PRO_VERSION=10.36.1

Also, check the [Jamf Docker Hub page](https://hub.docker.com/r/jamf/jamfpro/tags) to see if they've updated their jamfpro tag. The latest version as of 2022-03-10 is 0.0.16. If they've updated it since then and you want to update your image, change this value to reflect the new tag.

	JAMF_IMAGE_VERSION=0.0.16.

If you want to change the MySQL values, do so as well.

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

If you are using an Arm processor, then specify docker-compose-arm.yml.

```
docker compose -f docker-compose-arm.yml up -d
```

Open [http://localhost/](http://localhost/) and wait for it to startup. It can take several minutes. Follow the setup procedures (here's the [documentation](https://www.jamf.com/resources/product-documentation/) in case you need help). Accept the license agreement, enter your activation code, and create an account. For the Jamf Pro URL use "http://localhost" (for local testing only) or whatever your IP is.

## Roadmap

I am not using this in production yet. I plan on eventually using this to deploy my production server. As such, I plan on adding the following features.

- SSL support w/ signed certificates
- Custom server.xml
- jamf-cli support

Things I might do.

- Try to make it stable
- Automate the setup procedures (this would be for creating Jamf Pro test servers)
- Support Let's Encrypt
- Support Distribution Points

## Credit

This depends completely on the image provided by [Jamf](https://github.com/jamf/jamfpro). I had nothing to do with developing that image.

I used [Bryson Tyrrell's file](https://gist.github.com/brysontyrrell/95c9492f02a691b3f976830557f6d4ed) as a starting point for this project.

I am also using [wait-for-it.sh](https://github.com/vishnubob/wait-for-it/blob/81b1373f17855a4dc21156cfe1694c31d7d1792e/wait-for-it.sh) to pause Jamf until MySQL starts.

## License

MIT License
