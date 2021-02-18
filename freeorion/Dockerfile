ARG TZ
FROM ubuntu:20.04
ARG TZ
RUN apt update && DEBIAN_FRONTEND=noninteractive apt install tzdata

#ln -snf /usr/share/zoneinfo/America/Denver /etc/localtime && echo America/Denver > /etc/timezone

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN apt -y install software-properties-common && \
	add-apt-repository -y ppa:o01eg/freeorion-stable && \
	apt update && \
	apt -y install freeorion && \
	rm -rf /var/lib/apt/lists/*

RUN cp /usr/games/freeorion /usr/games/freeoriond && \
	sed -i "s|/usr/lib/freeorion/freeorion .*|/usr/lib/freeorion/freeoriond --hostless --network.server.unconn-human-empire-players.max 0 --network.server.conn-human-empire-players.min 0|g" /usr/games/freeoriond

ENTRYPOINT ["/usr/games/freeoriond"]
