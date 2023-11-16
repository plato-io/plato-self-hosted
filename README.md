# plato-self-hosted

## Store Plato's DB on your own DB server 

Plato ships with its own postgres container for storing application data. If you'd like to instead run this database on your own database server, you'll need to configure the `DATABASE_URL` environment variable, found [here](https://github.com/plato-io/plato-self-hosted/blob/main/docker-compose/docker-compose.yaml#L16) in docker-compose, and [here](https://github.com/plato-io/plato-self-hosted/blob/main/k8s/manifests/plato-service-api.yaml#L23) in k8s.

NOTE: Plato officially supports only Postgres 13.

Before running Plato, you must create and assign the `plato_api` role:

```sql
> CREATE ROLE plato_api;
> GRANT plato_api TO <your-plato-db-user>;
```
