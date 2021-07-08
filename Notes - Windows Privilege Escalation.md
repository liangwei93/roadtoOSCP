#### Service Exploits

Query the configuation of a service:
> sc.exe qc <name>

Query the current status of a service:
> sc.exe query <name>

Modify a configuration option of a service:
> sc.exe config <name> <option>= <value>

Start/Stop a service:
> net start/stop <name>


Service Misconfigurations
1. Insecure Service Properties
Each service has an ACL which defines certain service-specific permissions.
Some permissions are innocuous (SERVICE_QUERY_CONFIG, SERVICE_QUERY_STATUS)
Some may be useful (SERVICE_STOP, SERVICE_START)
Some are dangerous (SERVICE_CHANGE_CONFIG, SERVICE_ALL_ACCESS) 

Potential Rabbit Hold: Can change a service config but cannot stop/start the service, we cannot escalate privileges.

2. Unquoted Service Path

3. Weak Registry Permissions
4. Insecure Service Executables
5. DLL Hijacking
