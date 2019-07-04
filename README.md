# liferay-docker-portlet-ansible
Example project showing how to deploy a example portlet into a Dockerized Liferay with Ansible

Liferay is also available as Dockerized image (maintained by Liferay in https://github.com/liferay/liferay-docker): https://hub.docker.com/r/liferay/portal 

The Docker image provides several versions of Liferay, which is developed at https://github.com/liferay/liferay-portal

Documentation:

7.x: https://portal.liferay.dev/docs/7-2/user

6.x: https://portal.liferay.dev/docs/6-2/user


### Run Liferay 6.x & 7.x with Docker

The Dockerfile source is maintained on GitHub also: https://github.com/liferay/liferay-docker/blob/master/template/Dockerfile

##### Currently 6.x versions seem to be EE only and thus require a license

Wait, the file https://github.com/liferay/liferay-docker/blob/master/build_all_images.sh defines, which Version uses EE or CE. If one needs 6.x, there is `6.2.5-ga6`:

```
releases.liferay.com/portal/6.2.5-ga6/liferay-portal-tomcat-6.2-ce-ga6-20160112152609836.zip
```

Therefore let's try to use:

```
# 6.x
docker run -it -p 8080:8080 liferay/portal:6.2.5-ga6-201905012238
```

And voilà, having a look at http://localhost:8080/user/test/home should give you (after some configuration) a nice fully working CE version of Liferay.

##### 7.x 

This should look like 

```
2019-07-03 09:17:54.395 INFO  [main][DialectDetector:158] Using dialect com.liferay.portal.dao.orm.hibernate.HSQLDialect for HSQL Database Engine 2.3
2019-07-03 09:17:56.407 INFO  [main][ModuleFrameworkImpl:1334] Starting initial bundles
2019-07-03 09:18:03.413 INFO  [main][ModuleFrameworkImpl:1611] Started initial bundles
2019-07-03 09:18:03.417 INFO  [main][ModuleFrameworkImpl:1646] Starting dynamic bundles
2019-07-03 09:18:17.576 INFO  [main][ModuleFrameworkImpl:1743] Started dynamic bundles
2019-07-03 09:18:17.612 INFO  [main][ModuleFrameworkImpl:412] Navigate to Control Panel > Configuration > Gogo Shell and enter "lb" to see all bundles
2019-07-03 09:18:34.932 WARN  [Elasticsearch initialization thread][EmbeddedElasticsearchConnection:288] Liferay is configured to use embedded Elasticsearch as its search engine. Do NOT use embedded Elasticsearch in production. Embedded Elasticsearch is useful for development and demonstration purposes. Refer to the documentation for details on the limitations of embedded Elasticsearch. Remote Elasticsearch connections can be configured in the Control Panel.

    __    ____________________  _____  __
   / /   /  _/ ____/ ____/ __ \/   \ \/ /
  / /    / // /_  / __/ / /_/ / /| |\  /
 / /____/ // __/ / /___/ _, _/ ___ |/ /
/_____/___/_/   /_____/_/ |_/_/  |_/_/

Starting Liferay Community Edition Portal 7.1.3 CE GA4 (Judson / Build 7102 / May 14, 2019)

2019-07-03 09:18:52.477 INFO  [main][StartupHelper:72] There are no patches installed
Loading jar:file:/opt/liferay/tomcat-9.0.17/webapps/ROOT/WEB-INF/lib/portal-impl.jar!/portal.properties
2019-07-03 09:18:57.974 INFO  [main][AutoDeployDir:193] Auto deploy scanner started for /opt/liferay/deploy
2019-07-03 09:20:24.162 INFO  [main][ThemeHotDeployListener:108] 1 theme for admin-theme is available for use
2019-07-03 09:20:25.219 INFO  [main][ThemeHotDeployListener:108] 1 theme for classic-theme is available for use
2019-07-03 09:20:31.859 INFO  [main][SystemCheckOSGiCommands:54] System check is enabled. You can run a system check with the command "system:check" in Gogo shell.
03-Jul-2019 09:20:35.790 INFO [main] org.apache.catalina.startup.Catalina.start Server startup in [170,819] milliseconds
```

Have a look at http://localhost:8080/ to see running Liferay.


##### 7.x error integrity constraint violation: unique constraint or index violation: IX_C7057FF7

```
    __    ____________________  _____  __
   / /   /  _/ ____/ ____/ __ \/   \ \/ /
  / /    / // /_  / __/ / /_/ / /| |\  /
 / /____/ // __/ / /___/ _, _/ ___ |/ /
/_____/___/_/   /_____/_/ |_/_/  |_/_/

Starting Liferay Community Edition Portal 7.1.3 CE GA4 (Judson / Build 7102 / May 14, 2019)

2019-07-03 11:38:24.993 INFO  [main][StartupHelper:72] There are no patches installed
Loading jar:file:/opt/liferay/tomcat-9.0.17/webapps/ROOT/WEB-INF/lib/portal-impl.jar!/portal.properties
2019-07-03 11:38:31.145 INFO  [main][AutoDeployDir:193] Auto deploy scanner started for /opt/liferay/deploy
2019-07-03 11:39:47.029 INFO  [main][ThemeHotDeployListener:108] 1 theme for admin-theme is available for use
2019-07-03 11:39:47.950 INFO  [main][ThemeHotDeployListener:108] 1 theme for classic-theme is available for use
2019-07-03 11:39:55.341 INFO  [main][SystemCheckOSGiCommands:54] System check is enabled. You can run a system check with the command "system:check" in Gogo shell.
03-Jul-2019 11:40:01.981 INFO [main] org.apache.catalina.startup.Catalina.start Server startup in [165,153] milliseconds
2019-07-03 11:40:21.228 ERROR [http-nio-8080-exec-3][JDBCExceptionReporter:234] integrity constraint violation: unique constraint or index violation: IX_C7057FF7
2019-07-03 11:40:21.318 ERROR [http-nio-8080-exec-3][JDBCExceptionReporter:234] integrity constraint violation: unique constraint or index violation: IX_C7057FF7
2019-07-03 11:40:22.256 ERROR [http-nio-8080-exec-6][JDBCExceptionReporter:234] integrity constraint violation: unique constraint or index violation: IX_C7057FF7
2019-07-03 11:40:22.265 ERROR [http-nio-8080-exec-6][JDBCExceptionReporter:234] integrity constraint violation: unique constraint or index violation: IX_C7057FF7
/etc/liferay/entrypoint.sh: line 3:     6 Killed                  ${LIFERAY_HOME}/tomcat/bin/catalina.sh run
```

### Configure Liferay via portal.properties

https://github.com/liferay/liferay-portal/blob/master/portal-impl/src/portal.properties
