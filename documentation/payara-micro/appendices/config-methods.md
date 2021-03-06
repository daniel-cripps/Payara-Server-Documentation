# Configuration Methods
This section details the PayaraMicro.class configuration methods that are used during the bootstrap process.

Configuration Operand | Description | Get Method | Set Method | Default Value | Command Line Equivalent
--- | --- | --- | --- | --- | ---
Alternate Domain XML | Gets or sets the domain.xml used to override the default server configuration. | `File getAlternateDomainXML()` | ` PayaraMicro setAlternateDomainXML(File alternateDomainXML)` | `null` | `--domainConfig`
Cluster Multicast Group | Gets or sets the cluster multicast group for the instance. | `String getClusterMulticastGroup()` | `PayaraMicro setClusterMulticastGroup(String hzMulticastGroup)` | `null` (Default value of 224.2.2.4 set in default domain.xml is not read into instance variable) | `--mcAddress`
Cluster Port | Gets or sets the multicast cluster port. | `int getClusterPort()` | `PayaraMicro setClusterPort(int hzPort)` | -2147483648 (MIN_VALUE) (Default value of 2904 set in default domain.xml is not read into instance variable) | `--mcPort`
Cluster Start Port | Gets or sets the start port number for the Payara Micro instance to listen on for cluster communications. | `int getClusterStartPort()` | `PayaraMicro setClusterStartPort(int hzStartPort)` | -2147483648 (MIN_VALUE) (Default value of 5900 set in default configuration files is not read into instance variable) | `--startPort`
Deployment Directory | Gets or sets the directory to be scanned for archives to deploy. | `File getDeploymentDir()` | `PayaraMicro setDeploymentDir(File deploymentRoot)` | `null` | `--deploymentDir`
HTTP Port | Gets or sets the HTTP port for the instance to bind to. | `int getHttpPort()` | `PayaraMicro setHttpPort(int httpPort)` | -2147483648 (MIN_VALUE) (Default value of 8080 set in default domain.xml is not read into instance variable) | `--port`
Instance Name | Gets or sets the name of the instance. | `String getInstanceName()` | `PayaraMicro setInstanceName(String instanceName)` | Generated Universally Unique Identifier. | `--name`
Maximum HTTP Threads | Gets or sets the maximum number of threads in the HTTP thread pool. | `int getMaxHttpThreads()` | `PayaraMicro setMaxHttpThreads(int maxHttpThreads)` | -2147483648 (MIN_VALUE) (Default value of 10 set in default domain.xml is not read into instance variable) | `--maxHttpThreads`
Minimum HTTP Threads | Gets or sets the minimum number of threads in the HTTP thread pool. | `int getMinHttpThreads()` | `PayaraMicro setMinHttpThreads(int minHttpThreads)` | -2147483648 (MIN_VALUE) (Default value of 10 set in default domain.xml is not read into instance variable) | `--minHttpThreads`
Root Directory | Gets or sets the root configuartion directory. | `File getRootDir()` | `PayaraMicro setRootDir(File rootDir)` | `null` | `--rootDir`
HTTPS Port | Gets or sets the HTTPS port for the instance to bind to. A HTTPS port is not bound unless this value is manually set. | `int getSslPort()` | `PayaraMicro setSslPort(int sslPort)` | -2147483648 (MIN_VALUE) (Default value of 8443 set in default domain.xml is not read into instance variable) | `--sslPort`
No Clustering | Gets or sets whether clustering is enabled or disabled for an instance. | `boolean isNoCluster()` | `PayaraMicro setNoCluster(boolean noCluster)` | _false_ | 
HTTP Auto-Binding | Enables or Disables auto-binding of the HTTP port for an instance. | `boolean getHttpAutoBind()` | `PayaraMicro setHttpAutoBind(boolean httpAutoBind)` | _false_ | `--autoBindHttp`
HTTPS Auto-Binding | Enables or Disables auto-binding of the HTTPS port for an instance. | `boolean getSslAutoBind()` | `PayaraMicro setSslAutoBind(boolean sslAutoBind)` | _false_ | `--autoBindSsl`
Auto-Bind Range | Sets the range for HTTP and HTTPS port auto-binding. | `int getAutoBindRange()` | `PayaraMicro setAutoBindRange(int autoBindRange)` | 5 | `--autoBindRange`

