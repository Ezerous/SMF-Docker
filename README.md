# README

The docker compose files create two containers:

1. A container running PHP and Apache. PHP v5.4.45 is picked, as it seems that it doesn't produce errors while upgrading to SMF v2.0.x. If needed, however, a working PHP v5.3.29 edition of the images also exists.
2. A container running MySQL v5.1.73.

## Usage

- Use `docker compose -f compose-smf-1.1.0.yaml up -d` if you want an SMF v1.1.10 forum to be included (this uses named docker volumes).
- Use `docker compose -f compose-dev-env.yaml up -d` for an initial development environment. This will create two bind mounts locally, inside "volumes" directory.

For both cases, MySQL is hosted at `smf_db` with `root` as password of `root` user (feel free to edit).