# License Server for On-Premises Solutions

To enable on-premises deployment of our SDK, we have introduced a License Server that will handle SDK session authorization. This ensures that our product is used in compliance with the provided licenses and to prevent unauthorized usage.

# License Server Deployment

* We provide the server binaries (compiled according to your server platform) with built-in information about your license.
* You need to run it on your server (or your clients' servers) on the appropriate interface and port.
* TTo handle SSL connections, the License Server has to be run under a proxy server which will offload the SSL (e.g., Apache, Nginx, AWS balancers, etc.).
Nginx example:

```
server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    #Placeholder to include the ssl certificate and key
    ssl_certificate_key /etc/nginx/certs/ssl_certificate.key;
    location / {
        #proxy request to licence server
		proxy_pass http://127.0.0.1:3000/;
    }
}
```

# Configure SDK to use appropriate License Server

You need to provide the new license server endpoint to the SDK after the initialization:

```
const sdk = new window.tsvb({{CUSTOMER_ID}});
sdk.config({api_url: 'https://youdomain.com:port/sdk/session/'});
```

# License Server Configuration

Run the server:

```
./licensing-server --cfg=${CONFIG_PATH}
```

Example of configuration file:

```
# Example of Licensing Server configuration .env file.


# Path for service logs. Could be abosulute or relative.
LOG_PATH=./logs.log

# Logger will write anything that is on specified level or above. 
# Possible values: trace, debug, info, error, fatal, panic.
LOG_LEVEL=debug

# Maximum size in megabytes of the log file before it gets rotated.
# Default value: 100.
LOG_MAX_SIZE=1024

# Maximum number of days to retain old log files based on the timestamp
# encoded in their filename.
# The default is not to remove old log files based on age.
LOG_MAX_AGE=30

# Maximum number of old log files to retain.
# The default is to retain all old log files.
LOG_MAX_BACKUPS=30

# Determines if rotated log files should be compressed using gzip.
# Default value: false.
LOG_COMPRESS=true


# Path for storing statistics in JSON format. Could be abosulute or relative.
STATS_PATH=./stats.json

# Maximum size in megabytes of the statistics file before it gets rotated.
# Default value: 100.
STATS_MAX_SIZE=1024

# Maximum number of days to retain statistics log files based on the timestamp
# encoded in their filename.
# The default is not to remove old statistics files based on age.
STATS_MAX_AGE=30

# Maximum number of old statistics files to retain.
# The default is to retain all old statistics files.
STATS_MAX_BACKUPS=30

# Determines if rotated statistics files should be compressed using gzip.
# Default value: false.
STATS_COMPRESS=true


# HTTP API interface.
# Default value: 0.0.0.0.
API_INTERFACE=127.0.0.1

# HTTP API port.
# Default value: 80.
API_PORT=3000

```

# Logs Rotation:

To enforce a log file be rotated, you should pass the SIGUSR1 signal to a process:

```
kill -SIGUSR1 ${PID}
```

# Statistic Rotation

To enforce a statistics file be rotated, you should pass the SIGUSR2 signal to a process:

```
kill -SIGUSR2 ${PID}
```