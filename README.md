# Testsuite

Testsuite for the HAL management console based on [Drone & Graphene](http://arquillian.org/guides/functional_testing_using_graphene/) Arquillian extensions.

## Profiles

The testsuite uses various profiles to decide how and which tests to run. The following profiles are available:

- `chrome` | `firefox` | `safari`: Defines the browser to run the tests (mutual exclusive)
- `basic`, `rbac`, `transaction`: Defines which tests to run (can be combined)
- `standalone` | `domain`: Defines the operation mode (mutual exclusive)

Combine multiple profiles to define your setup. Choose at least one profile from each line. Please note that you cannot combine profiles which are marked as mutual exclusive. 

Valid combinations:

- `chrome,basic,standalone`
- `firefox,basic,rbac,domain`
- `safari,rbac,transaction,standalone`

Invalid combinations:

- `safari,firefox`
- `basic,transaction`
- `standalone,domain`
- `chrome,basic,standalone,domain`

## Run Tests 

In order to run tests you need a running WildFly / JBoss EAP server with an insecure management interface. Use the following commands to remove the security realm from the management interface:

**Standalone**

```
/core-service=management/management-interface=http-interface:undefine-attribute(name=security-realm)
:reload
```

**Domain**

```
/host=master/core-service=management/management-interface=http-interface:undefine-attribute(name=security-realm)
/host=master:reload
```

Run all tests:

```bash
mvn test -P<profiles>
```

Run a single test: 

```bash
mvn test -P<profiles> -Dtest=<fully qualified classname>
```

To debug the test(s) use the `maven.surefire.debug` property: 
 
```bash
mvn test -P<profiles> -Dtest=<fully qualified classname> -Dmaven.surefire.debug
```

The tests will automatically pause and await a remote debugger on port 5005. You can then attach to the running tests using your IDE. 