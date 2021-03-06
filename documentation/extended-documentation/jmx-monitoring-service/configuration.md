# Configuring the JMX Monitoring Service
_Payara Server and Micro 163 (4.1.1.163) onwards_

There are two possible ways to configure the JMX Monitoring Service:

1. Using the `set-monitoring-configuration` asadmin command
2. Editing the `domain.xml` file

Examples on how to use the service to monitor the HeapMemoryUsage attribute using both methods are shown below, but it is first worth noting the default configuration values for the service:

* _enabled_: `false`, valid type: `Boolean`
* _amx_: `false`, valid type: `Boolean`, _optional_
* _logfrequency_: `15`, valid type: `Long`, _optional_
* _logfrequencyunit_: `SECONDS`, valid type: `TimeUnit`, _optional_

Monitoring attributes are added to the service as properties of the configuration and contain the following values:

* _name_: the MBean attribute name
* _value_: the MBean domain name
* _description_: displayed in the `get-monitoring-configuration` asadmin command, *optional*

## 1. Using the asadmin command

### Adding the monitoring attribute

To add HeapMemoryUsage to the list of MBean attributes to monitor using the service the following command can be used:

```shell
asadmin> set-monitoring-configuration --addproperty 'name=HeapMemoryUsage value=java.lang:type=Memory' --enabled false
```

Breaking this command down, two options have been used:

* `--addproperty`
* `--enabled`

Passing `--addproperty` to `set-monitoring-configuration` provides a way to add a new MBean attribute to monitor using the service. The option takes in a string of space-delimited key-value pairs corresponding to the values listed earlier. The `name` and `value` fields are required, but `description` is not. Providing `name=HeapMemoryUsage` denotes that the name of the MBean attribute to log is `HeapMemoryUsage`, while `value=java.lang:type=Memory` denotes the `ObjectName` of the MBean to look for the attribute on is `java.lang:type=Memory`.

The second option passed, `--enabled`, is the only required option for the asadmin command. The only valid values to give this option are `true` or `false`. Passing `false` to the option will disable the logging service on next startup if it is currently enabled, but will otherwise do nothing. Under this scenario the monitoring service has not been configured yet so `false` was passed to the option.

#### Dealing with composite MBean attributes

The MBean attribute added, `HeapMemoryUsage`, is a composite attribute. It has metrics for `commited`, `init`, `max` and `used`. The monitoring service will by default monitor each metric and log it as `{$metric}{$attribute_name}:{$attribute_value}`.

If this is not the desired result, it is possible to monitor a single metric for a composite MBean attribute. To monitor a single metric for the attribute the value of `name` passed to the `--addproperty` option should be modified like so:

```shell
name=HeapMemoryUsage.metric
```

So to log only the used heap memory the asadmin command would be:

```shell
asadmin> set-monitoring-configuration --addproperty 'name=HeapMemoryUsage.used value=java.lang:type=Memory' --enabled false
```

### Setting logging frequency

There are two configuration attributes related to the frequency at which log messages are written, `logfrequency` and `logfrequencyunit`. The first of the two is a numerical value for the rate, while the second value is the unit for the rate. The default configuration is set to have a message logged every 15 seconds.

If the value of `logfrequencyunit` is the default of `SECONDS` then to have the monitoring service log messages every one minute execute the command:

```shell
asadmin> set-monitoring-configuration --logfrequency 60 --enabled false
```

### Enabling the monitoring service

After configuring the monitoring service, there are two options to enable it. The service can either be enabled for next startup or the service can be dynamically enabled on a running instance of Payara (provided a non-empty configuration existed at server startup). To enable the service dynamically on the default running instance of Payara the command to run is:

```shell
asadmin> set-monitoring-configuration --dynamic true --enabled true
```

To enable the service for next startup the `--dynamic` option would need to be dropped from the command.

## 2. Editing the `domain.xml` file

To configure the monitoring service through editing the `domain.xml` file it's useful to know about the structure of the monitoring service's tag.

```xml
<monitoring-service-configuration enabled="true" logfrequency="60">
  <property name="Attribute1" value="MBean1"></property>
  <property name="Attribute2" value="MBean2"></property>
</monitoring-service-configuration>
```

The `<monitoring-service-configuration>` tag houses the configuration values in its attributes. Omitted values take the respective default value. If the configuration is edited while the server is running the service must be restarted for the configuration changes to updates.

If the service has not yet run on the instance then the configuration tag will not have been created. To manually create it the tag needs to be added to the `domain.xml` in the relevant config section.

```xml
<configs>
  <config name="server-config">
    ...
    <monitoring-service-configuration>
    </monitoring-service-configuration>
    ...
  </config>
</configs>
```

### Adding the monitoring attribute

MBean attributes to monitor are added to the configuration section as `<property>` tags. Each property tag can take values for `name`, `value` and `description`. To add an MBean attribute to monitor a `<property>` tag should be added as shown:

```xml
<monitoring-service-configuration>
  <property name="HeapMemoryUsage" value="java.lang:type=Memory"></property>
</monitoring-service-configuration>
```

Here all of the necessary attributes have been given to the tag, `name` and `value`. The value given to `name` should be the name of the MBean attribute to monitor, while the value given to `value` should be the `ObjectName` of the MBean to look for the MBean attribute on. Here the MBean attribute added is `HeapMemoryUsage` which resides in the MBean with the `ObjectName` of `java.lang:type=Memory`.

#### Dealing with composite MBean attributes

The MBean attribute added, `HeapMemoryUsage`, is a composite attribute. It has metrics for `commited`, `init`, `max` and `used`. The monitoring service will by default monitor each metric and log it as `{$metric}{$attribute_name}:{$attribute_value}`.

If this is not the desired result, it is possible to monitor a single metric for a composite MBean attribute. To monitor a single metric for the attribute the attribute of `name` for the property should be changed to:

```xml
name="HeapMemoryUsage.metric"
```

So to log only the used heap memory the configuration would looked like this:

```xml
<monitoring-service-configuration>
  <property name="HeapMemoryUsage.used" value="java.lang:type=Memory"></property>
</monitoring-service-configuration>
```

### Setting logging frequency

There are two configuration attributes related to the frequency at which log messages are written, `logfrequency` and `logfrequencyunit`. The first of the two is a numerical value for the rate, while the second value is the unit for the rate. The default configuration is set to have a message logged every 15 seconds.

To have the monitoring service log messages every one minute change the tag as shown:

```xml
<monitoring-service-configuration logfrequency="60">
  <property name="HeapMemoryUsage" value="java.lang:type=Memory"></property>
</monitoring-service-configuration>
```

### Enabling the monitoring service

Now that the service is configured, it can be enabled simply by adding `enabled="true"` to the tag.

```xml
<monitoring-service-configuration enabled="true" logfrequency="60">
  <property name="HeapMemoryUsage" value="java.lang:type=Memory"></property>
</monitoring-service-configuration>
```

Saving the `domain.xml` will result in the monitoring service enabled on next startup to log heap memory usage every minute. To activate on a running instance of Payara the asadmin command should be used to enable the service, with the `--dynamic` option as shown above.
