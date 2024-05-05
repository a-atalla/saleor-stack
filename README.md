# Saleor Stack

Starting with the [Saleor Platform](https://github.com/saleor/saleor-platform)
as a foundation, setting up a single-server Saleor configuration tailored for
production use by small to medium-sized businesses.

## Development

you can use this repo for development locally the same way you use
saleor-platofrm, the database migration will be done for you via `db-init`
container, you only need to populate the database with example data.

```
docker compose run --rm api python3 manage.py populatedb
```

## Production use

- Change `DEBUG` env variable to `False`.
- Adjust `.env` for your need by changing `SECRET_KEY` and
  `DJANGO_SUPERUSER_XXXX` values, also you should go through
  [Environment variables](https://docs.saleor.io/docs/3.x/setup/configuration)
  for more configurations.
- You need to generate
  [RSA private key](https://docs.saleor.io/docs/3.x/setup/configuration#rsa_private_key)
  for jwt sigining and set the env variable `RSA_PRIVATE_KEY`.
- Remove `mailpit` service from the docker-compose file, update the
  `DEFAULT_FROM_EMAIL` and `EMAIL_URL` in `.env` to actual values.
- Make sure to pin the docker images tags to avoid unexpected behaviour before
  running for production.
- You may need to use
  [Database Read Replicas](https://docs.saleor.io/docs/3.x/setup/read-replicas)
  to allow Saleor to offload the primary database by moving the read-only
  traffic to a separate database, the postgresql readonly user will be created
  for you.
- It is recommended to use
  [Jaeger](https://docs.saleor.io/docs/3.x/setup/monitoring-jaeger) to monitor
  on production.
- If USA/USD is not your country/currency, before adding any data you will need
  to delete the default channel and create a new channel with the required
  country/currency
