# rethinkdb-dockerfiles

Dockerfiles for past and present versions of RethinkDB.

[![](https://badge.imagelayers.io/anapsix/rethinkdb:latest.svg)](https://imagelayers.io/?images=anapsix/rethinkdb:latest)

## Usage

You may pass additional arguments to the container as you would to RethinkDB server binary.

    docker run -it --rm anapsix/rethinkdb --help

Starting 2 node cluster on 2 hosts

    # on host host01.acmecorp.com
    docker run -d --name rethinkdb \
      -p 28015:28015 -p 29015:29015 -p 8080 \
      anapsix/rethinkdb \
      --bind all \
      --join host01.acmecorp.com --join host02.acmecorp.com \
      --canonical-address $(curl -sS icanhazip.com)

    # on host host02.acmecorp.com
    docker run -d --name rethinkdb \
      -p 28015:28015 -p 29015:29015 -p 8080 \
      anapsix/rethinkdb \
      --bind all --join host01.acmecorp.com --join host02.acmecorp.com \
      --canonical-address $(curl -sS icanhazip.com)

> It is save to pass it's own hostname to RethinkDB, since it ignores connections to self.
> When starting a cluster, you should pass one (or multiple) `--canonical-address` directive(s) to
customize which HOST/IP RethinkDB uses to advertise self to other hosts.
> Otherwise, it won't start.

## Tutum deployment

[![Deploy to Tutum](https://s.tutum.co/deploy-to-tutum.svg)](https://dashboard.tutum.co/stack/deploy/)

Start script is now [Tutum](http://tutum.co) compatible.
All is needed to start a 2+ node cluster is do a one-to-one port mapping 
(8080->8080, 28015->28015, 29015->29015) and that's it.
All instances will auto-discover each other and join the cluster.
Go to service enpoint for port 8080 to access web UI.

## Documentation

https://github.com/docker-library/docs/blob/master/rethinkdb/README.md

## Procedure for updating

1. Realize RethinkDB has updated. *This part needs improving.*
2. Add new version to `versions` array in `./generate_dockerfile.sh`
3. Run `./generate_dockerfile.sh`
4. Commit and push changes
