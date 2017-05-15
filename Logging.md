# Logging



## Strategy

- It is beneficial to use an UUID to denote a transaction process, so that it will be more clear for developer to check the operation for a single process.
- print is not a good idea, because compared with logging functions:
  - You can not control message level and filter out not important ones
  - You can not decide where and how to output later
- ​



## Language

### Python

`logging` 提供root级别的log

`logger` 提供root级别下，特定element 级别的访问

```python
import logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)
```

 By giving different level to logger or handler, you can write only error messages to specific log file, or record debug details when debugging. 



**Logger Name**

`logger = logging.getLogger(__name__)`



**Capture exceptions and record them with traceback**

By calling logger methods with exc_info=True parameter, traceback is dumped to the logger

```python
try:
    open('/path/to/does/not/exist', 'rb')
except (SystemExit, KeyboardInterrupt):
    raise
except Exception, e:
    logger.error('Failed to open file', exc_info=True)
```



**Get logger when need it**

It’s better to get the logger when you need it. It’s cheap to create or get a logger. 



**Use JSON or YAML logging configuration**

``` yaml
version: 1
disable_existing_loggers: False
formatters:
    simple:
        format: "%(asctime)s - %(name)s - %(levelname)s - %(message)s"

handlers:
    console:
        class: logging.StreamHandler
        level: DEBUG
        formatter: simple
        stream: ext://sys.stdout

    info_file_handler:
        class: logging.handlers.RotatingFileHandler
        level: INFO
        formatter: simple
        filename: info.log
        maxBytes: 10485760 # 10MB
        backupCount: 20
        encoding: utf8

    error_file_handler:
        class: logging.handlers.RotatingFileHandler
        level: ERROR
        formatter: simple
        filename: errors.log
        maxBytes: 10485760 # 10MB
        backupCount: 20
        encoding: utf8

loggers:
    my_module:
        level: ERROR
        handlers: [console]
        propagate: no

root:
    level: INFO
    handlers: [console, info_file_handler, error_file_handler]
```



``` python
import os
import logging.config

import yaml

def setup_logging(
    default_path='logging.yaml',
    default_level=logging.INFO,
    env_key='LOG_CFG'
):
    """Setup logging configuration

    """
    path = default_path
    value = os.getenv(env_key, None)
    if value:
        path = value
    if os.path.exists(path):
        with open(path, 'rt') as f:
            config = yaml.safe_load(f.read())
        logging.config.dictConfig(config)
    else:
        logging.basicConfig(level=default_level)
```



**Use rotating file handler**

`RotatingFileHandler` instead of `FileHandler` in production environment.



**Setup a central log server when you have multiple servers**

