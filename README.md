# Tor Proxy

Spawn a Tor proxy in one single command.

# Setup

Firstly, install both [Docker Engine and Docker Compose](https://docs.docker.com/get-docker/)

Once done, clone this repository :

```bash
git clone git@github.com:Ximaz/tor-proxy
```

then jump into the project's folder and build the image :

```bash
cd tor-proxy
docker compose build
```

Finally, you can spawn a container by running the following command :

```bash
docker compose up
```

You also could spawn a new container by running a raw

```bash
docker docker run --rm -it -p9050-9054 --entrypoint tor tor-proxy
```

command, but you don't benefit easily of the capabilities management.

# Usage

Once you spawned a Tor proxy, you should be able to access it using `cURL`.

Firstly, ensure that your Tor proxy is ready to accept connections with the
following command :

```bash
[[ $(
        curl "https://check.torproject.org/" --socks5 "127.0.0.1:9050" -s | \
        grep "Congratulations. This browser is configured to use Tor."
    ) != "" ]] \
&& echo "The proxy is ready" \
|| echo "You are not connected to a Tor proxy"
```

If everything is fine, you should see `The proxy is ready`. If you see anything
else, or if the load time is way too long, your proxy is probably not up yet.

Once your proxy is ready, you can abuse your proxy :

```bash
curl -X GET "https://example.com" --socks5 "127.0.0.1:9050"
```

# Security

As mentionned above, this project helps you to both spawn a new tor proxy AND
manage it's capabilities.

Effectively, the Tor proxy listens on the following ports :
- `9050`,
- ~~`9051`~~ (dedicated),
- `9052`,
- `9053`,
- `9054`

But it's binded to the `0.0.0.0` interface which makes it accessible from
anywhere.

That's where capabilities come in.

If you're running a Tor proxy locally, you probably should not be afraid of
something bad happening. But in a scenario where you're trying to host proxies
over the Internet, if your containers get compromised, it should not cause
severe issues.

Another thing to consider, even though it's not really an explicit security
protection, is that the current Alpine image used for this project has nothing
installed excepted the Tor process itself. It's both updated and upgraded, and
the latest Alpine image is used.

# Disclaimer

Do not use this project for illegal / unethical actions. I will not be held
responsible for your bad behaviour.
