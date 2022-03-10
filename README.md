# This repository contains my Dockerfiles

## Directory structure

- [jamfpro](./jamfpro) My attempt at a [Jamf Pro](https://www.jamf.com/) server.
- [freeorion](./freeorion) My attempt at a [FreeOrion](https://freeorion.org/) hostless server.

## Really useful one-liners

### Apache

Start apache and serve the current directory.

	docker run --rm -it -p8080:80 -v $(pwd):/usr/local/apache2/htdocs httpd

### asciicast2gif

	docker run --rm -v $PWD:/data asciinema/asciicast2gif https://asciinema.org/a/diFmnNEo94uA65B0HMu205B1w.cast demo.gif

### Jekyll

Build and serve Jekyll

	docker run --rm -it -v $PWD:/srv/jekyll jekyll/builder jekyll build
	docker run --rm -it -p4000:4000 -v $PWD:/srv/jekyll jekyll/builder jekyll serve

## zsh function to run docker one-liners

Add the following to your ~/.zshrc file. This has the ability to specify port and directory. On macOS it will also open a web browser to localhost.

	docker_fun() {
		port=$1
		if [ -n "$3" ]; then
			port=$3
		fi
		dir=$PWD
		if [ -n "$4" ]; then
			if [ -d "/$4" ]; then
				dir=$4
			else
				echo "\"$4\" must be an absolute path (use \$PWD/ to get your current directory)."
				return
			fi
		fi
		echo $dir
		if [ $(uname) = "Darwin" ]; then
			open "http://localhost:$port" &
		fi
		echo docker run --rm -it -p$port:$2 -v"$dir":$5 $6 $7
		docker run --rm -it -p$port:$2 -v"$dir":$5 $6 $7
	}

	apache() {
		docker_fun 8080 80 "$1" "$2" /usr/local/apache2/htdocs httpd
	}

	jekyll() {
		echo 1 $1 2 $2
		docker_fun 4000 4000 "$1" "$2" /srv/jekyll jekyll/builder /bin/bash
	}

Change to the directory you want to be the site root and then run this.

	apache

To specify a port

	apache 8180

To specify a port and directory

	apache 8180 $PWD/Sites

## License

Copyright (c) 2021 James Reynolds - MIT License
