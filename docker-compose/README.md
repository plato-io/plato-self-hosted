# Configuration

Plato's version, public HTTP port and other settings could be specified in `.env` file.
By default, latest version would be started and Plato will listen on port `8080`.

# Start

License key should be specified in `.env` file. It can be also passed during startup:

```
PLATO_LICENSE_KEY="…" docker compose up --wait
```

When command returns Plato is ready to use.
