# Meowthpxnk python logging util

## Logger installing

1. Create configuration file `config/logging.yaml`
   You can configurate with default formatters and loggers in python default logger.

```yml
version: 1
disable_existing_loggers: False

formatters:
    simple:
        (): mpxnk_logging.logging.formatters.BaseFormatter
        settings:
            module: True

    colorized:
        (): mpxnk_logging.logging.formatters.ColorisedFormatter
        settings:
            module: True
            log_path: True

    colorized.module:
        (): mpxnk_logging.logging.formatters.ColorisedFormatter
        settings:
            module: True

handlers:
    console:
        class: logging.StreamHandler
        level: DEBUG
        formatter: colorized
        stream: ext://sys.stdout

    console.module:
        class: logging.StreamHandler
        level: DEBUG
        formatter: colorized.module
        stream: ext://sys.stdout

    file:
        class: logging.FileHandler
        level: INFO
        formatter: simple
        filename: LOGGING.LOG

loggers:
    uvicorn:
        level: INFO
        handlers: [console.module, file]

    alembic:
        level: INFO
        handlers: [console.module, file]

    app:
        level: INFO
        handlers: [console, file]

    app.api:
        level: ERROR
```

2. Install yaml

```sh
poetry add pyyaml
```

3. Create yaml reading function

```py
def read_file(path: str) -> str:
    with open(path, encoding="utf-8") as file:
        return file.read()


def read_yaml(path: str) -> dict:
    return yaml.safe_load(read_file(path))
```

4. Setup logger

```py
LOGGING_CONFIG = read_yaml("config/logging.yaml")
logging.config.dictConfig(LOGGING_CONFIG)
```

5. Gratz! You can test your logs

```py
logger = logging.getLogger("app")
logger.info("Startup application")
```

---

$XOXO$
_meowthpxnk_
..
