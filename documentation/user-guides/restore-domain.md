# Restore a Payara Server Domain

To restore from a backup the asadmin `restore-domain` command is used:

```
asadmin restore-domain --filename ${name} --backupdir /path/to/backup/directory myDomain
```

This command will restore just the domain named in the command. Since instances are stored separately to each domain (and may be on remote hosts), **the command will have no effect on instances**.

### Restoring an Instance

Since all data in an instance directory is kept in sync with the DAS, there is no added data (besides logs, caches and some MQ data when a file-based store is used) and an instance can be completely restored simply by making sure there is an empty directory with the name of the instance inside a node folder of the correct name. For example: `payara41/nodes/localhost-localdomain/myInstance`

Running the `start-local-instance` command with a full sync, as discussed earlier, will restore the instance completely:

```
asadmin start-local-instance --sync=full myInstance
```

### Considerations for OpenMQ
If OpenMQ (Payara Server’s JMS broker) is being used, the default configuration is to use `EMBEDDED` mode, meaning that Payara Server will fully manage the JMS broker within the same JVM as the server instance and a file-based store is used for persistent messages.

The alternative option of `LOCAL` allows for the use of an “enhanced” broker cluster, whereby the JMS broker instances are started as separate JVMs and use a database connection for storage. Assuming the database is highly available, this completely avoids any danger when needing to `--sync=full` the instance.

----

#### See Also

* [Payara Server Domain Backup](backup-domain.md)
* [Upgrade Payara Server](upgrade-payara.md)