Reproducer for `pom.xml` system property handling of `quarkus.application.version`.

Run `quarkus dev` or `./mvnw quarkus:dev`, then in another terminal issue a `GET /hello` request. The response should include the `pom.xml` version as a lowercased string.

```bash
$ curl -v http://localhost:8080/hello
* Host localhost:8080 was resolved.
* IPv6: ::1
* IPv4: 127.0.0.1
*   Trying [::1]:8080...
* connect to ::1 port 8080 from ::1 port 52112 failed: Connection refused
*   Trying 127.0.0.1:8080...
* Connected to localhost (127.0.0.1) port 8080
> GET /hello HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/8.9.1
> Accept: */*
> 
* Request completely sent off
< HTTP/1.1 200 OK
< content-length: 38
< Content-Type: text/plain;charset=UTF-8
< 
* Connection #0 to host localhost left intact
Hello from Quarkus REST 1.0.0-snapshot
```

Upgrade to Quarkus 3.20 LTS: `quarkus up -y -S 3.20` and repeat the test. This now fails:

```
$ curl -v http://localhost:8080/hello
... (trimmed long output)

<h3>The stacktrace below is the original. <a href="" onClick="toggleStackTraceOrder(); return false;">See the stacktrace in reversed order</a> (root-cause first)</h3>        <code class="stacktrace"><pre>io.vertx.core.impl.NoStackTraceException
Caused by: io.quarkus.dev.appstate.ApplicationStartException: java.lang.RuntimeException: Failed to start quarkus
	at io.quarkus.dev.appstate.ApplicationStateNotification.waitForApplicationStart(ApplicationStateNotification.java:63)
	at io.quarkus.runner.bootstrap.StartupActionImpl.runMainClass(StartupActionImpl.java:142)
	at io.quarkus.deployment.dev.IsolatedDevModeMain.restartApp(IsolatedDevModeMain.java:202)
	at io.quarkus.deployment.dev.IsolatedDevModeMain.restartCallback(IsolatedDevModeMain.java:183)
	at io.quarkus.deployment.dev.RuntimeUpdatesProcessor.doScan(RuntimeUpdatesProcessor.java:555)
	at io.quarkus.deployment.dev.RuntimeUpdatesProcessor.doScan(RuntimeUpdatesProcessor.java:455)
	at io.quarkus.vertx.http.runtime.devmode.VertxHttpHotReplacementSetup$6.call(VertxHttpHotReplacementSetup.java:163)
	at io.quarkus.vertx.http.runtime.devmode.VertxHttpHotReplacementSetup$6.call(VertxHttpHotReplacementSetup.java:150)
	at io.vertx.core.impl.ContextImpl.lambda$executeBlocking$4(ContextImpl.java:192)
	at io.vertx.core.impl.ContextInternal.dispatch(ContextInternal.java:270)
	at io.vertx.core.impl.ContextImpl$1.execute(ContextImpl.java:221)
	at io.vertx.core.impl.WorkerTask.run(WorkerTask.java:56)
	at org.jboss.threads.ContextHandler$1.runWith(ContextHandler.java:18)
	at org.jboss.threads.EnhancedQueueExecutor$Task.doRunWith(EnhancedQueueExecutor.java:2675)
	at org.jboss.threads.EnhancedQueueExecutor$Task.run(EnhancedQueueExecutor.java:2654)
	at org.jboss.threads.EnhancedQueueExecutor$ThreadBody.run(EnhancedQueueExecutor.java:1591)
	at org.jboss.threads.DelegatingRunnable.run(DelegatingRunnable.java:11)
	at org.jboss.threads.ThreadLocalResettingRunnable.run(ThreadLocalResettingRunnable.java:11)
	at io.netty.util.concurrent.FastThreadLocalRunnable.run(FastThreadLocalRunnable.java:30)
	at java.base/java.lang.Thread.run(Thread.java:1583)
Caused by: java.lang.RuntimeException: Failed to start quarkus
	at io.quarkus.runner.ApplicationImpl.doStart(Unknown Source)
	at io.quarkus.runtime.Application.start(Application.java:101)
	at io.quarkus.runtime.ApplicationLifecycleManager.run(ApplicationLifecycleManager.java:121)
	at io.quarkus.runtime.Quarkus.run(Quarkus.java:77)
	at io.quarkus.runtime.Quarkus.run(Quarkus.java:48)
	at io.quarkus.runtime.Quarkus.run(Quarkus.java:137)
	at io.quarkus.runner.GeneratedMain.main(Unknown Source)
	at java.base/jdk.internal.reflect.DirectMethodHandleAccessor.invoke(DirectMethodHandleAccessor.java:103)
	at java.base/java.lang.reflect.Method.invoke(Method.java:580)
	at io.quarkus.runner.bootstrap.StartupActionImpl$1.run(StartupActionImpl.java:116)
	... 1 more
Caused by: jakarta.enterprise.inject.spi.DeploymentException: io.quarkus.runtime.configuration.ConfigurationException: Failed to load config value of type class java.lang.String for: quarkus.application.version

	at io.quarkus.arc.runtime.ConfigRecorder.validateConfigProperties(ConfigRecorder.java:70)
	at io.quarkus.runner.recorded.ConfigBuildStep$validateRuntimeConfigProperty1282080724.deploy_0(Unknown Source)
	at io.quarkus.runner.recorded.ConfigBuildStep$validateRuntimeConfigProperty1282080724.deploy(Unknown Source)
	... 11 more
	Suppressed: java.util.NoSuchElementException: SRCFG00011: Could not expand value app.imageVersionLower in property quarkus.application.version
		at io.smallrye.config.ExpressionConfigSourceInterceptor$1.accept(ExpressionConfigSourceInterceptor.java:94)
		at io.smallrye.config.ExpressionConfigSourceInterceptor$1.accept(ExpressionConfigSourceInterceptor.java:71)
		at io.smallrye.common.expression.ExpressionNode.emit(ExpressionNode.java:22)
		at io.smallrye.common.expression.Expression.evaluateException(Expression.java:56)
		at io.smallrye.common.expression.Expression.evaluate(Expression.java:70)
		at io.smallrye.config.ExpressionConfigSourceInterceptor.getValue(ExpressionConfigSourceInterceptor.java:71)
		at io.smallrye.config.ExpressionConfigSourceInterceptor.getValue(ExpressionConfigSourceInterceptor.java:45)
		at io.smallrye.config.SmallRyeConfig$SmallRyeConfigSourceInterceptorContext.proceed(SmallRyeConfig.java:1230)
		at io.smallrye.config.FallbackConfigSourceInterceptor.getValue(FallbackConfigSourceInterceptor.java:24)
		at io.smallrye.config.SmallRyeConfig$SmallRyeConfigSourceInterceptorContext.proceed(SmallRyeConfig.java:1230)
		at io.smallrye.config.SmallRyeConfig.getConfigValue(SmallRyeConfig.java:495)
		at io.smallrye.config.inject.ConfigProducerUtil.lambda$getConfigValue$2(ConfigProducerUtil.java:170)
		at io.smallrye.config.SecretKeys.doUnlocked(SecretKeys.java:28)
		at io.smallrye.config.inject.ConfigProducerUtil.getConfigValue(ConfigProducerUtil.java:170)
		at io.smallrye.config.inject.ConfigProducerUtil.getValue(ConfigProducerUtil.java:91)
		at io.quarkus.arc.runtime.ConfigRecorder.validateConfigProperties(ConfigRecorder.java:60)
		... 13 more
Caused by: io.quarkus.runtime.configuration.ConfigurationException: Failed to load config value of type class java.lang.String for: quarkus.application.version

	... 14 more</pre></code>
    </div>
    <div id="reversed-stacktrace" class="trace hidden">
<h3>The stacktrace below has been reversed to show the root cause first. <a href="" onClick="toggleStackTraceOrder(); return false;">See the original stacktrace</a></h3>        <code class="stacktrace"><pre>io.quarkus.runtime.configuration.ConfigurationException: Failed to load config value of type class java.lang.String for: quarkus.application.version

	at io.quarkus.arc.runtime.ConfigRecorder.validateConfigProperties(ConfigRecorder.java:70)
	at io.quarkus.runner.recorded.ConfigBuildStep$validateRuntimeConfigProperty1282080724.deploy_0(Unknown Source)
	at io.quarkus.runner.recorded.ConfigBuildStep$validateRuntimeConfigProperty1282080724.deploy(Unknown Source)
	at io.quarkus.runner.ApplicationImpl.doStart(Unknown Source)
	at io.quarkus.runtime.Application.start(Application.java:101)
	at io.quarkus.runtime.ApplicationLifecycleManager.run(ApplicationLifecycleManager.java:121)
	at io.quarkus.runtime.Quarkus.run(Quarkus.java:77)
	at io.quarkus.runtime.Quarkus.run(Quarkus.java:48)
	at io.quarkus.runtime.Quarkus.run(Quarkus.java:137)
	at io.quarkus.runner.GeneratedMain.main(Unknown Source)
	at java.base/jdk.internal.reflect.DirectMethodHandleAccessor.invoke(DirectMethodHandleAccessor.java:103)
	at java.base/java.lang.reflect.Method.invoke(Method.java:580)
	at io.quarkus.runner.bootstrap.StartupActionImpl$1.run(StartupActionImpl.java:116)
	at java.base/java.lang.Thread.run(Thread.java:1583)
Resulted in: jakarta.enterprise.inject.spi.DeploymentException: io.quarkus.runtime.configuration.ConfigurationException: Failed to load config value of type class java.lang.String for: quarkus.application.version

	... 14 more
Resulted in: java.lang.RuntimeException: Failed to start quarkus
	... 11 more
Resulted in: io.quarkus.dev.appstate.ApplicationStartException: java.lang.RuntimeException: Failed to start quarkus
	at io.quarkus.dev.appstate.ApplicationStateNotification.waitForApplicationStart(ApplicationStateNotification.java:63)
	at io.quarkus.runner.bootstrap.StartupActionImpl.runMainClass(StartupActionImpl.java:142)
	at io.quarkus.deployment.dev.IsolatedDevModeMain.restartApp(IsolatedDevModeMain.java:202)
	at io.quarkus.deployment.dev.IsolatedDevModeMain.restartCallback(IsolatedDevModeMain.java:183)
	at io.quarkus.deployment.dev.RuntimeUpdatesProcessor.doScan(RuntimeUpdatesProcessor.java:555)
	at io.quarkus.deployment.dev.RuntimeUpdatesProcessor.doScan(RuntimeUpdatesProcessor.java:455)
	at io.quarkus.vertx.http.runtime.devmode.VertxHttpHotReplacementSetup$6.call(VertxHttpHotReplacementSetup.java:163)
	at io.quarkus.vertx.http.runtime.devmode.VertxHttpHotReplacementSetup$6.call(VertxHttpHotReplacementSetup.java:150)
	at io.vertx.core.impl.ContextImpl.lambda$executeBlocking$4(ContextImpl.java:192)
	at io.vertx.core.impl.ContextInternal.dispatch(ContextInternal.java:270)
	at io.vertx.core.impl.ContextImpl$1.execute(ContextImpl.java:221)
	at io.vertx.core.impl.WorkerTask.run(WorkerTask.java:56)
	at org.jboss.threads.ContextHandler$1.runWith(ContextHandler.java:18)
	at org.jboss.threads.EnhancedQueueExecutor$Task.doRunWith(EnhancedQueueExecutor.java:2675)
	at org.jboss.threads.EnhancedQueueExecutor$Task.run(EnhancedQueueExecutor.java:2654)
	at org.jboss.threads.EnhancedQueueExecutor$ThreadBody.run(EnhancedQueueExecutor.java:1591)
	at org.jboss.threads.DelegatingRunnable.run(DelegatingRunnable.java:11)
	at org.jboss.threads.ThreadLocalResettingRunnable.run(ThreadLocalResettingRunnable.java:11)
	at io.netty.util.concurrent.FastThreadLocalRunnable.run(FastThreadLocalRunnable.java:30)
	... 1 more
Resulted in: io.vertx.core.impl.NoStackTraceException</pre></code>
    </div>
<div id="stacktrace"></div></div></body>
</html>
```
