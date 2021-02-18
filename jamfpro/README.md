# Jamf Pro docker-compose

Docker Compose file to get [Jamf Pro](https://www.jamf.com/) running in a container.

FYI: Jamf Pro is not free and is not included in this repository. You must purchase it before this will work. Instructions for downloading it are below.

IMPORTANT: Because the Dockerfile in this repository creates an image with the Jamf Pro application included, you should *never* upload it to [Docker Hub](https://hub.docker.com/) or else you might get lawyers calling you. ¯\\_(ツ)_/¯

Download and install docker-compose and [docker](https://www.docker.com/get-started).

## Clone Repository

```
git clone github.com/magnusviri/dockerfiles.git
cd dockerfiles/jamfpro
cp .env_example .env
```

## Download Jamf Pro

Login to [https://www.jamf.com/jamf-nation/my/assets](https://www.jamf.com/jamf-nation/my/assets), find the version of Jamf Pro you want to download, select "Manual" then click "Download For Manual". Save your "Activation Code" since you will need to enter it once the server starts up.

Find the jamf-pro-installation-x.x.x.zip file you downloaded, and unzip it. Take the ROOT.war file that was unzipped and put it in the directory that represents the version. That is, if it's Jamf Pro 10.26.1, put it in a folder named 10.26.1 inside of this project. Like this.

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

## Environment Variables

Edit the .env so that JAMF_PRO_VERSION is the correct version.

	JAMF_PRO_VERSION=10.26.1

Also, check the [JamfDevops Docker Hub page](https://hub.docker.com/r/jamfdevops/jamfpro/tags?page=1&ordering=last_updated) to see if they've updated their jamfpro tag. This was made with 0.0.12. If they've updated it and you want to update yours, change this value to reflect the new tag.

	JAMF_IMAGE_VERSION=0.0.12.

If you want to change the MySQL values, do so as well.

## Persistent MySQL Data or Not

If you don't want persistent MySQL data, then open up docker-compose.yml and comment out the following two lines.

>     volumes:
>       - ./mysql-data:/var/lib/mysql

## Change the Port

This will open up port 80 on your computer. If you want it on a different port, open the docker-compose.yml file and change the "80" in the following lines to something else.

>     ports:
>       - "80:8080"

I tried to set this via an environment variable but it didn't work. Maybe I had the syntax wrong.

## Start

```
docker-compose up -d
```

Open [http://localhost/](http://localhost/) and wait for it to startup. Follow the setup procedures (here's the [documentation](https://www.jamf.com/resources/product-documentation/) in case you need help). Accept the license agreement, enter your activation code, and create an account. For the Jamf Pro URL use "http://localhost" (for local testing only) or whatever your IP is.

## Debugging

When debugging this, you can quickly undo everything by running this command (after `docker-compose up -d`).

	docker-compose down ; docker image rm jamfpro:10.26.1 ; docker image rm jamfdevops/jamfpro:0.0.12 ; rm -r mysql-data/*

## Roadmap

I plan on eventually using this to deploy my production server. As such, I plan on adding the following features.

- SSL support w/ signed certificates
- Switch to MySQL 8

Things I might do.

- Automate the setup procedures (this would be for creating Jamf Pro test servers)
- Support Let's Encrypt
- Support Distribution Points

Please let me know if there's anything I'm missing.

## Credit

None of this would be possible without the awesome image provided by [Jamf](https://github.com/jamf/jamfpro).

I used [Bryson Tyrrell's file](https://gist.github.com/brysontyrrell/95c9492f02a691b3f976830557f6d4ed) as a starting point for this project.

## License

MIT License
