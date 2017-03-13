ansible-role-influxdb
=========

[![Build Status](https://travis-ci.org/knowhy/ansible-role-influxdb.svg?branch=master)](https://travis-ci.org/knowhy/ansible-role-influxdb)

Ansible role to install InfluxDB database.

This role deploys a single InfluxDB instance. The role uses the InfluxDB release artifacts from [influxDB homepage](https://dl.influxdata.com/influxdb/releases).

Most of the configuration is done in the `influxdb.conf` file. `defaults/main.yml` contains the default values from the config shipped with the release artifact.

Changes to default configuration
--------------------------------

- changed Systemd Type from `notify` to `fork`. The default `notify` setting does not seem to work.

Requirements
------------

- none

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

    influxdb_version: 1.2.0

InfluxDB version to deploy.

    influxdb_user: root
	influxdb_group: root

The user and group under which InfluxDB server will run. Defaults to `influxdb`.

	influxdb_data_path: /var/lib/influxdb

The path under which InfluxDB will store its data.

	influxdb_log_path: /var/log/influxdb

The paths under which InfluxDB will store its logs.

	influxdb_log_stdout_enabled: True
	influxdb_log_stdout_file: influxdb.out
	influxdb_log_stderr_enabled: True
	influxdb_log_stderr_file: influxdb.err

Whether to enable `stdout` and `stderr` logs and how to configure the filenames for the log files.

	influxdb_reporting_disabled: True

Boolean, whether to send anonymous data to m.influxdb.com, defaults to `False`.

	influxdb_use_hostname: True

Whether to let influxdb resolv its hostname, defaults to `False`.

	influxdb_hostname: localhost

Meta data directory to use, defaults to `/var/lib/influxdb/meta`.

	influxdb_meta_data_dir: /var/lib/influxdb/meta

Meta data retention autocreate, boolean, defaults to `True`.

	influxdb_meta_retention_autocreate: True

Whether to enable meta data logging, boolean, defaults to `True`.

	influxdb_meta_logging_enabled: True

Directory to store data, defaults to `/var/lib/influxdb/data`.

	influxdb_data_dir: /var/lib/influxdb/data

Directory for the `WAL` storage engine, defaults to `/var/lib/influxdb/wal`.

	influxdb_data_wal_dir: "/var/lib/influxdb/wal"

Trace logging provides more verbose output around the tsm engine. Turning this on can provide more useful output for debugging tsm engine issues. Boolean, defaults to `False`.

	influxdb_data_trace_logging_enabled: False

Whether queries should be logged before execution, boolean, defaults to `False`.

	influxdb_data_data_query_log_enabled: False

The maximum size a shard's cache can reach before it starts rejecting writes, defaults to `524288000`.

	influxdb_data_cache_max_memory_size: 524288000

The size at which the engine will snapshot the cache and write it to a TSM file, freeing up memory, defaults to `26214400`.

	influxdb_data_cache_snapshot_memory_size: 26214400

The length of time at which the engine will snapshot the cache and write it to a new TSM file if the shard hasn't received writes or deletes, defaults to `1h`.

	influxdb_data_cache_snapshot_write_cold_duration: 1h

The duration at which the engine will compact all TSM files in a shard if it hasn't received a write or delete, defaults to `24h`.

	influxdb_data_compact_full_write_cold_duration: 24h

The maximum series allowed per database before writes are dropped. This limit can prevent high cardinality issues at the database level. This limit can be disabled by setting it to 0.

	influxdb_data_max_series_per_database: 1000000

The maximum number of tag values per tag that are allowed before writes are dropped. This limit can prevent high cardinality tag values from being written to a measurement. This limit can be disabled by setting it to `0`. Defaults to `100000`.

	influxdb_data_max_values_per_tag: 100000

The default time a write request will wait until a "timeout" error is returned to the caller.

	influxdb_coordinator_write_timeout: 10s

The maximum number of concurrent queries allowed to be executing at one time.  If a query is executed and exceeds this limit, an error is returned to the caller.  This limit can be disabled by setting it to 0.

	influxdb_coordinator_max_concurrent_queries: 0

The maximum time a query will is allowed to execute before being killed by the system.  This limit can help prevent run away queries.  Setting the value to 0 disables the limit.

	influxdb_coordinator_query_timeout: 0s

The the time threshold when a query will be logged as a slow query.  This limit can be set to help discover slow or resource intensive queries.  Setting the value to 0 disables the slow query logging.

	influxdb_coordinator_log_queries_after: 0s

The maximum number of points a SELECT can process.  A value of 0 will make the maximum point count unlimited.

	influxdb_coordinator_max_select_point: 0

The maximum number of series a SELECT can run.  A value of 0 will make the maximum series count unlimited.

	influxdb_coordinator_max_select_series: 0

The maxium number of group by time bucket a SELECt can create.  A value of zero will max the maximum number of buckets unlimited.

	influxdb_coordinator_max_select_buckets: 0

Controls the enforcement of retention policies for evicting old data, boolean, defaults to `True`.

	influxdb_retention_enabled: True

Interval for retention check, defaults to `30m`.

	influxdb_retention_check_interval: 30m

Controls the precreation of shards, so they are available before data arrives, boolean, set to `True` in default config, defaults to `False`

	influxdb_shard_precreation_enabled: False

Check interval for shard precreation, defaults to `10m`.

	influxdb_shard_precreation_check_interval: 10m

Advance period for shard precreation, defaults to `30m`.

	influxdb_shard_precreation_advance_period: 30m

Whether to record statistics internally, boolean, defaults to `True`.

	influxdb_monitor_store_enabled: True

The destination database for recorded statistics, defaults to `_internal`.

	influxdb_monitor_store_database: "_internal"

 The interval at which to record statistics, defaults to `10s`.

	influxdb_monitor_store_interval: 10s

Controls the availability of the built-in, web-based admin interface, boolean, defaults to `True`.

	influxdb_admin_enabled: True

Bind address and port for the admin interface, defaults to `:8083`.

	influxdb_admin_bind_address: ":8083"

Whether to enable https for the admin interface, boolean, defaults to `False`.

	influxdb_admin_https_enabled: False

Path to the https certificate for the admin interface, defaults to `/etc/ssl/influxdb.pem`.

	influxdb_admin_https_certificate: "/etc/ssl/influxdb.pem"

Whether to enable http endpoint, boolean, defaults to `True`.

	influxdb_http_enabled: True

Bind address and port for the http endpoint, defaults to `:8086`.

	influxdb_http_bind_address: ":8086"

Whether to enable authentification for the http endpoint, boolean, defaults to `False`.

	influxdb_http_auth_enabled: False

Whether to enable logging for the http endpoint, boolean, defaults to `True`.

	influxdb_http_log_enabled: True

The default realm sent back when issuing a basic auth challenge. Defaults to `InfluxDB`.

  influxdb_http_realm: InfluxDB

Whether to enable write tracing for the http endpoint, boolean, defaults to `False`.

	influxdb_http_write_tracing: False

Whether to enable pprof for the http endpoint, defaults to `False`.

	influxdb_http_pprof_enabled: False

Whether to enable TLS for the http endpoint, boolean, defaults to `False`.

	influxdb_http_https_enabled: False

Path to certificate for TLS for the http endpoint, defaults to `/etc/ssl/influxdb.pem`.

	influxdb_http_https_certificate: "/etc/ssl/influxdb.pem"

Use a separate private key location.

	influxdb_http_https_private_key: []

The JWT auth shared secret to validate requests using JSON web tokens.

	influxdb_http_https_shared_secret: []

Max row limit for the http endpoint, defaults to `10000`.

	influxdb_http_max_row_limit: 10000

The maximum number of HTTP connections that may be open at once.  New connections that would exceed this limit are dropped.  Setting this value to `0` disables the limit. Defaults to `0`.

	influxdb_http_max_connection_limit: 0

Enable http service over unix domain socket. Boolean. Defaults to `False`.

	influxdb_http_unix_socket_enabled: False

The path of the unix domain socket. Defaults to `/var/run/influxdb.sock`.

	influxdb_http_bind_socket: /var/run/influxdb.sock

Determines whether the subscriber service is enabled. Boolean. Defaults to `True`.

	influxdb_subscriber_enabled: True

The default timeout for HTTP writes to subscribers. Defaults to `30s`.

	influxdb_subscriber_http_timeout: 30s

Allows insecure HTTPS connections to subscribers. This is useful when testing with self-signed certificates. Boolean. Defaults to `False`.

	influxdb_subscriber_insecure_skip_verify: False

The path to the PEM encoded CA certs file. If the empty string, the default system certs will be used. Defaults to `[]`.

	influxdb_subscriber_ca_certs: []

The number of writer goroutines processing the write channel. Defaults to `40`.

	influxdb_subscriber_write_concurrency: 40

The number of in-flight writes buffered in the write channel. Defaults to `1000`.

	influxdb_subscriber_write_buffer_size: 1000

Wheter to enable Graphite, boolean, defaults to `False`.

	influxdb_graphite_enabled: False

Graphite database to use, defaults to `graphite`.

	influxdb_graphite_database: graphite

Graphite bind addresss, defaults to `:2003`.

	influxdb_graphite_bind_address: ":2003"

Graphite protocol, defaults to `tcp`.

	influxdb_graphite_protocol: tcp

Graphite consistency level, defaults to `one`.

	influxdb_graphite_consistency_level: one

Graphite batching will flush if this many points get buffered, defaults to `5000`.

	influxdb_graphite_batch_size: 5000

Graphite number of batches that may be pending in memory, defaults to `10`.

	influxdb_graphite_batch_pending: 10

Graphite will flush at least this often even if we haven't hit buffer limit, defaults to `1s`.

	influxdb_graphite_batch_timeout: 1s

Graphite UDP Read buffer size, `0` means OS default, UDP listener will fail if set above OS max, defaults to `0`.

	influxdb_graphite_udp_read_buffer: 0

Graphite string to join multiple matching 'measurement' values providing more control over the final measurement name, defaults to `.`.

	influxdb_graphite_separator: "."

Graphite default tags that will be added to all metrics. These can be overridden at the template level or by tags extracted from metric. List. Defaults to `["region=us-east", "zone=1c"]`.

	influxdb_graphite_tags:
	 - region=us-east
	 - zone=1c

Graphite templates. Each template line requires a template pattern.  It can have an optional filter before the template and separated by spaces.  It can also have optional extra tags following the template.  Multiple tags should be separated by commas and no spaces similar to the line protocol format.  There can be only one default template.

	influxdb_graphite_templates:
  - "*.app env.service.resource.measurement"
  - "server.*" # Default template

Whether to enable collectd, boolean, defaults to `False`.

	influxdb_collectd_enabled: False

Bind address for collectd, defaults to ` `.

	influxdb_collectd_bind_address: ""

Collectd database to use, defaults to ` `.

	influxdb_collectd_database: ""

Retention policy for collectd. Defaults to `[]`.

	influxdb_collectd_retention_policy: []

Collect typesdb to use, defaults to ` `.

	influxdb_collectd_typesdb: ""

Security level for collectd. Defaults to `none`.

	influxdb_collectd_security_level: none

Auth file for collectd. Defaults to `/etc/collectd/auth_file`.

	influxdb_collectd_auth_file: /etc/collectd/auth_file

Collectd batch size, will flush if this many points get buffered, defaults to `1000`.

	influxdb_collectd_batch_size: 1000

Collectd number of batches that may be pending in memory, defaults to `5`.

	influxdb_collectd_batch_pending: 5

Collectd batch timeout, will flush at least this often even if we haven't hit buffer limit, defaults to `1s`.

	influxdb_collectd_batch_timeout: 1s

Collectd UDP Read buffer size, 0 means OS default. UDP listener will fail if set above OS max, defaults to `0`.

	influxdb_collectd_read_buffer: 0

Whether to enable OpenTSDB, boolean, defaults to `False`.

	influxdb_opentsdb_enabled: False

OpenTSDB bind address, defaults to `:4242`.

	influxdb_opentsdb_bind_address: ":4242"

OpenTSDB database, defaults to `opentsdb`.

	influxdb_opentsdb_database: opentsdb

OpenTSDB retention policy, defaults to ` `.

	influxdb_opentsdb_retention-policy: ""

OpenTSDB consistency level, defaults to `one`.

	influxdb_opentsdb_consistency-level: one

Whether to enable TLS for OpenTSDB, boolean, defaults to `False`.

	influxdb_opentsdb_tls-enabled: False

Path to certificate for TLS for OpenTSDB, defaults to ` `.

	influxdb_opentsdb_certificate: ""

Whether to log an error for every malformed point, boolean, defaults to `True`.

	influxdb_opentsdb_log-point-errors: True

OpenTSDB batch size, will flush if this many points get buffered, defaults to `1000`.

	influxdb_opentsdb_batch_size: 1000

Number of batches that may be pending in memory, defaults to `5`.

	influxdb_opentsdb_batch_pending: 5

Timeout for OpenTSDB, will flush at least this often even if we haven't hit buffer limit, defaults to `1s`.

	influxdb_opentsdb_batch_timeout: "1s"

Whether to enable UDP listener, boolean, defaults to `False`.

	influxdb_udp_enabled:  False

Bind address for UDP listener, defaults to ` `.

	influxdb_udp_bind_address: ""

InfluxDB database for UDP, defaults to `udp`.

	influxdb_udp_database: udp

Retention policy for UDP, defaults to ` `.

	influxdb_udp_retention_policy: ""

Batch size for UDP, defaults to `1000`.

	influxdb_udp_batch_size: 1000

Number of batches that may be pending in memory, defaults to `5`.

	influxdb_udp_batch_pending: 5

Batch timeout for UDP, will flush at least this often even if we haven't hit buffer limit, defaults to `1s`.

	influxdb_udp_batch_batch_timeout: "1s"

UDP Read buffer size, `0` means OS default. UDP listener will fail if set above OS max, defaults to `0`.

	influxdb_udp_read_buffer: 0

Whether to enable continuous queries log, boolean, defaults to `True`.

	influxdb_continuous_queries_log_enabled: True

Whether to enable continuous queries, boolean, defaults to `True`.

	influxdb_continuous_queries_enabled: True

Run interval for continuous queries, interval for how often continuous queries will be checked if they need to run, defaults to `1s`.

	influxdb_continuous_queries_run_interval: "1s"

Dependencies
------------

none

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: knowhy.influxdb }

License
-------

agpl-3.0

Author Information
------------------

This role was created in 2016 by Henrik Pingel.
