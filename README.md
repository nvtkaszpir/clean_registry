# clean_registry
Clean the Docker Registry by removing untagged repositories and running the garbage collector in Docker Registry >= 2.4.0

The optional ``-x`` flag may be used to remove the specified repositories or tagged images.

# WARNING:

Make backups of target docker-registry to avoid losing data.

This script stops the Registry container during cleanup to prevent corruption, making it temporarily unavailable to clients.

This script assumes the [filesystem](https://github.com/docker/distribution/blob/master/docs/configuration.md#storage) storage driver.

## Running standalone

This script may be run as stand-alone (on local setups) or dockerized (which supports both local and remote Docker setups). To run stand-alone, the best is to run in virtualenv and install required packages via pip:

```bash
virtualenv --python=python3 .venv
. .venv/bin/activate
pip3 install --upgrade pip
pip3 install -r requirements.txt
```

You may need to execute above commands as privileged user to access docker service (sudo + activate virtualenv).

## Usage

```
clean_registry.py [OPTIONS] CONTAINER [REPOSITORY[:TAG]]...
Options:
        -x, --remove    Remove the specified images or repositories.
        -q, --quiet     Suppress non-error messages.
        -V, --version   Show version and exit.
```

## Docker usage with local Docker setup

```bash
docker run --rm --volumes-from CONTAINER -v /var/run/docker.sock:/var/run/docker.sock ricardobranco/clean_registry [OPTIONS] CONTAINER [REPOSITORY[:TAG]] ...
```

## Docker usage with remote Docker setup

Make sure to read about [remote Docker setup](https://docs.docker.com/engine/security/https/#secure-by-default).

```bash
docker run --rm --volumes-from CONTAINER -e DOCKER_HOST -e DOCKER_TLS_VERIFY=1 -v /root/.docker:/root/.docker ricardobranco/clean_registry [OPTIONS] CONTAINER [REPOSITORY[:TAG]]...
```

Note:

Paths other than ``/root/.docker`` path may be specified with the ``DOCKER_CERT_PATH`` environment variable.  In any case, your ``~/.docker/*.pem`` files should be in the server to be able to run as a client against itself.
