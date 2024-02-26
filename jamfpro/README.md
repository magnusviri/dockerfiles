# Jamf Pro docker-compose

This is a docker–compose file to get [Jamf Pro](https://www.jamf.com/) running in a container. I have been using this to easily test [jctl](https://github.com/univ-of-utah-marriott-library-apple/jctl) against many different versions of Jamf Pro, which I run locally on my M1 laptop. I wanted to run my production server this way but alas, I never setup a production Docker server so I only use this for testing.

FYI: Jamf Pro is not free and is not included in this repository. You must purchase it before this will work. And even if you purchase Jamf, you will want to request a development license. Request the development license from your [Jamf Account Team](https://account.jamf.com/account-team) (login required).

This docker-compose file mounts the Jamf Pro ROOT.war file. It also mounts the mysql data folder. Each of these is stored in a folder named after the version. Specify the version in the .env file. This allows you to keep around old versions and easily switch.

## Slack

If you have questions or want to discuss this please do it on the jamf-docker channel on the [MacAdmins Slack](https://www.macadmins.org/).

## Install

Download and install [Docker](https://www.docker.com/get-started).

### Clone Repository and Create Environment File

```
git clone github.com/magnusviri/dockerfiles.git
cd dockerfiles/jamfpro
cp .env_example .env
```

### Download Jamf Pro

Login to [https://account.jamf.com](https://account.jamf.com) and then go to the  [Jamf Pro download page](https://account.jamf.com/products/jamf-pro/download). Below the "Download" button, click on the text "for Linux" and then select "Download For Manual". Then click the "Download" button. 

Under the "[Instances](https://account.jamf.com/products/jamf-pro)" tab, copy your "Activation Code" and save it in a note. You'll need to enter it once the server starts up.

In the Finder, find the jamf-pro-installation-x.x.x.zip file you downloaded, and unzip it. Take the ROOT.war file that was unzipped and put it in the directory that represents the version. That is, if it's Jamf Pro 11.3.0, put it in a folder named 11.3.0 inside of the data folder in this project, like this.

```
> ls -lAR
total 64
-rw-r--r--  1 james  staff   584 Feb 26 10:09 .env
-rw-r--r--  1 james  staff   176 Feb 26 10:10 .env_example
-rw-r--r--  1 james  staff    22 Feb 26 09:57 .gitignore
-rw-r--r--  1 james  staff  4882 Feb 26 10:12 README.md
drwxr-xr-x  4 james  staff   128 Feb 26 10:17 data
-rw-r--r--  1 james  staff  1034 Feb 26 10:04 docker-compose.yml
-rwxr-xr-x  1 james  staff  5227 Jul 27  2023 wait-for-it.sh

./data:
total 8
drwxr-xr-x  3 james  staff   96 Feb 26 10:09 11.3.0
-rw-r--r--  1 james  staff  107 Dec 13 15:26 my license.txt

./data/11.3.0:
total 824992
-rw-r--r--  1 james  staff  422395884 Feb 13 10:28 ROOT.war
```

### Environment Variables

Edit the .env so that JAMF_PRO_VERSION is the correct version.

	JAMF_PRO_VERSION=11.3.0

Also, check the [Jamf Docker Hub page](https://hub.docker.com/r/jamf/jamfpro/tags) to see if they've updated their jamfpro tag. The latest version as of 2024-02-26 is 0.0.22. If they've updated it since then and you want to update your image, change this value to reflect the new tag.

	JAMF_IMAGE_VERSION=0.0.22.

If you want to change the MySQL values, do so as well. This repo specifies "latest."

Note, Jamf is using features that prevent it from working with MariaDB.

By default, this docker-compose will open up port 80 on your computer. If you want it on a different port, change the following line to something else.

	JAMF_PRO_PORT=80

### Start

```
docker compose up -d
```

Open [http://localhost/](http://localhost/) and wait for it to startup. It can take several minutes. Follow the setup procedures (here's the [documentation](https://www.jamf.com/resources/product-documentation/) in case you need help). Accept the license agreement, enter your activation code, and create an account. For the Jamf Pro URL use "http://localhost" (for local testing only) or whatever your IP is.

## Roadmap

As long as I am working on [jctl](https://github.com/univ-of-utah-marriott-library-apple/jctl) I will keep this repo updated and working.

I am not using this in production and don't plan to. Because of this, I am no longer planning on adding new features. If anyone would like to contribute or get this production ready, I would certainly update my repo with those changes. Here's what it would take.

- SSL support w/ signed certificates
- Custom server.xml
- jamf-cli support
- Support Let's Encrypt
- Support Distribution Points

## Credit

This depends completely on the image provided by [Jamf](https://github.com/jamf/jamfpro). I had nothing to do with developing that image.

I used [Bryson Tyrrell's file](https://gist.github.com/brysontyrrell/95c9492f02a691b3f976830557f6d4ed) as a starting point for this project.

I am also using [wait-for-it.sh](https://github.com/vishnubob/wait-for-it/blob/81b1373f17855a4dc21156cfe1694c31d7d1792e/wait-for-it.sh) to pause Jamf until MySQL starts.

## License

MIT License
