# Jamf Pro docker-compose

Docker Compose file to get [Jamf Pro](https://www.jamf.com/) running in a container.

FYI: Jamf Pro is not free and is not included in this repository. You must purchase it before this will work. Instructions for downloading it are below.

2021-06-21 update: This docker-compose file no longer creates a docker image that includes Jamf Pro. Instead it mounts the Jamf Pro ROOT.war file in a generic image. This saves a lot of disk space and time.

## Slack

If you have questions or want to discuss this please do it on the jamf-docker channel on the [MacAdmins Slack](https://www.macadmins.org/).

## Install

Download and install docker-compose and [docker](https://www.docker.com/get-started).

### Clone Repository and Create Environment File

```
git clone github.com/magnusviri/dockerfiles.git
cd dockerfiles/jamfpro
cp .env_example .env
```

### Download Jamf Pro

Login to [https://www.jamf.com/jamf-nation/my/assets](https://www.jamf.com/jamf-nation/my/assets), find the version of Jamf Pro you want to download, select "Manual" then click "Download For Manual". Save your "Activation Code" since you will need to enter it once the server starts up.

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

	JAMF_PRO_VERSION=10.26.1

Also, check the [JamfDevops Docker Hub page](https://hub.docker.com/r/jamfdevops/jamfpro/tags?page=1&ordering=last_updated) to see if they've updated their jamfpro tag. The latest version as of 2021-06-21 is 0.0.13. If they've updated it since then and you want to update your image, change this value to reflect the new tag.

	JAMF_IMAGE_VERSION=0.0.13.

If you want to change the MariaDB values, do so as well.

### Change the Port

This will open up port 80 on your computer. If you want it on a different port, open the docker-compose.yml file and change the "80" in the following lines to something else.

	ports:
	  - "80:8080"

### Start

```
docker-compose up -d
```

Open [http://localhost/](http://localhost/) and wait for it to startup. It can take several minutes. Follow the setup procedures (here's the [documentation](https://www.jamf.com/resources/product-documentation/) in case you need help). Accept the license agreement, enter your activation code, and create an account. For the Jamf Pro URL use "http://localhost" (for local testing only) or whatever your IP is.

## Roadmap

I am not using this in production yet because I see it crash a lot. I plan on eventually using this to deploy my production server. As such, I plan on adding the following features.

- SSL support w/ signed certificates

Things I might do.

- Try to make it stable
- Automate the setup procedures (this would be for creating Jamf Pro test servers)
- Support Let's Encrypt
- Support Distribution Points

## Credit

This depends completely on the image provided by [Jamf](https://github.com/jamf/jamfpro). I had nothing to do with developing that image.

I used [Bryson Tyrrell's file](https://gist.github.com/brysontyrrell/95c9492f02a691b3f976830557f6d4ed) as a starting point for this project.

## License

MIT License
