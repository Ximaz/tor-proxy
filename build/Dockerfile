FROM alpine:3.20.1

LABEL maintainer="malo.durand@epitech.eu"

RUN apk update

RUN apk upgrade

RUN apk add --no-cache tor

COPY ./torrc /etc/tor/torrc

EXPOSE 9050

EXPOSE 9052

EXPOSE 9053

EXPOSE 9054

USER tor

ENTRYPOINT [ "tor" ]
