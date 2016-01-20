Zabbix.NET
===================


**Zabbix.NET** is .NET Zabbix API library which is incredible easy to use thanks to C# dynamic objects. 
There is no need for special classes for zabbix API methods. You can pass parameters via the anonymous objects. Or just your regular objects.

Usage:
```
Zabbix zabbix = new Zabbix(user, pass, zabbixUrl);
zabbix.Login();
string resultStr = zabbix.jsonResponse("host.get", new {output: "extend"});
Response resultObj = zabbix.objectResponse("host.get", new {output: "extend"});
zabbix.Logout();
```

Response class
```
public string jsonrpc { get; set; }
public dynamic result = new ExpandoObject();
public int id { get; set; }
```



Example (Log all hosts with active trigger):
```
Zabbix zabbix = new Zabbix("yourapiuser", "yourpassword", "http://yourzabbix.domain.eu/zabbix/api_jsonrpc.php");

zabbix.login();
Response responseObj = zabbix.objectResponse("trigger.get", new
	{ output = new string[] {"hostname", "description", "lastchange", "priority", "value", "status", "triggerid" },
      min_severity = 3,
      expandData = true,
      expandDescription = true,
      expandExpression = true,
      selectHosts = "extend",
      selectGroups = "extend",
      monitored = true,
      sortfield = "hostname",
      skipDependent = true,
      filter = new {value = 1 }
     });
zabbix.logout();

foreach (dynamic data in responseObj.result)
{
	Console.WriteLine(data.Hostname);
}
```



Note that you pass API method to Zabbix.jsonRespopnse (objectResponse) method as a string.

> **Note:**

> - You need to use login() method after creating Zabbix class and logout() after you are done with the API to make sure there are no open sessions left. 
> - Zabbix.NET uses Newtonsoft.Json library for json serialization and deserialization
> - .NET 4.0, but you can compile to any version of .NET as long as it supports ExpandoObject and Newtonsoft.Json 
> - Now available on NuGET **PM> Install-Package Zabbix.NET**