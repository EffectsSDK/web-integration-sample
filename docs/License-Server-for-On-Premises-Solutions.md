# License Server for On-Premises Solutions

To enable on-premises deployment of our SDK, we have introduced a License Server that will handle SDK session authorization. This ensures that our product is used in compliance with the provided licenses and to prevent unauthorized usage.

The License Server is entirely self-contained and operates without any dependencies or the need for internet access. It can function entirely on a local level.

# License Server Deployment

* We provide the server binaries (compiled according to your server platform, most commonly it's Debian based linux) with built-in information about your license.
* You need to run it on your server (or your clients' servers) on the appropriate interface and port.
* You can readily encapsulate it within a Docker container (or execute it within containers on different servers).
* To handle SSL connections, the License Server has to be run under a proxy server which will offload the SSL (e.g., Apache, Nginx, AWS balancers, etc.).
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

# Performance and throughput

The License Server demonstrates impressive performance, with a single instance capable of processing between 50,000 to 80,000 requests per second, depending on the available hardware.

Furthermore, to enhance redundancy and increase throughput, you can effortlessly deploy multiple VPS/VDS/containers behind a load balancer, tailored to your specific requirements.

You can begin with the following hardware configuration as a starting point: 2 VCPU (2.00GHz) and 1-2GB of RAM, which should be sufficient to manage up to 20,000 requests per second.

For enhanced performance, it is recommended to improve performance by deactivating detailed logging. You can achieve this by configuring the following setting:

```
LOG_LEVEL=panic
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

The server has the capability to gather statistics on SDK usage and save them to a local log file. By default, all SDK statistics are deactivated. You can activate them as necessary by configuring the appropriate flags.

To enforce a statistics file be rotated, you should pass the SIGUSR2 signal to a process:

```
kill -SIGUSR2 ${PID}
```
