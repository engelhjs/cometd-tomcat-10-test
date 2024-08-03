# cometd-tomcat-test

Project to test CometD on Tomcat 10.

With Cometd 7, handshake requests work fine.

With CometD 8, a handshake request results in the following exception:

```json
[
  {
    "requestId": "tuhwdhpsvzigotjmrbqtxpcb",
    "id": "4",
    "version": "1.0",
    "minimumVersion": "1.0",
    "channel": "/meta/handshake",
    "supportedConnectionTypes": [
      "long-polling",
      "callback-polling"
    ],
    "advice": {
      "timeout": 60000,
      "interval": 0
    }
  }
]
```

Exception:

```
org.cometd.server.HttpException: java.lang.IllegalStateException: In non-blocking mode you may not write to the ServletOutputStream until the previous write has completed and isReady() returns true
	at org.cometd.server.http.AbstractHttpTransport$1.fail(AbstractHttpTransport.java:185)
	at org.cometd.server.http.AbstractHttpTransport$Writer.onCompleteFailure(AbstractHttpTransport.java:758)
	at org.eclipse.jetty.util.IteratingCallback.processing(IteratingCallback.java:335)
	at org.eclipse.jetty.util.IteratingCallback.iterate(IteratingCallback.java:231)
	at org.cometd.server.http.AbstractHttpTransport.write(AbstractHttpTransport.java:213)
	at org.cometd.server.http.AbstractHttpTransport.flush(AbstractHttpTransport.java:437)
	at org.cometd.server.http.AbstractHttpTransport.lambda$processMessages$1(AbstractHttpTransport.java:244)
	at org.cometd.bayeux.Promise$3.succeed(Promise.java:119)
	at org.cometd.common.AsyncFoldLeft$AbstractLoop.run(AsyncFoldLeft.java:237)
	at org.cometd.common.AsyncFoldLeft.run(AsyncFoldLeft.java:107)
	at org.cometd.server.http.AbstractHttpTransport.processMessages(AbstractHttpTransport.java:242)
	at org.cometd.server.http.JSONHttpTransport.process(JSONHttpTransport.java:87)
	at org.cometd.server.http.JSONHttpTransport$UTF8Reader.finish(JSONHttpTransport.java:195)
	at org.cometd.server.http.JSONHttpTransport$UTF8Reader.onEOF(JSONHttpTransport.java:188)
	at org.cometd.server.http.JSONHttpTransport$AbstractReader.processEOF(JSONHttpTransport.java:156)
	at org.cometd.server.http.JSONHttpTransport$AbstractReader.onDataAvailable(JSONHttpTransport.java:140)
	at org.cometd.server.http.JSONHttpTransport$AbstractReader.read(JSONHttpTransport.java:107)
	at org.cometd.server.http.jakarta.JakartaCometDRequest$JakartaCometDInput.onDataAvailable(JakartaCometDRequest.java:159)
	at org.apache.coyote.Request.onDataAvailable(Request.java:272)
	at org.apache.catalina.connector.CoyoteAdapter.asyncDispatch(CoyoteAdapter.java:204)
	at org.apache.coyote.AbstractProcessor.dispatch(AbstractProcessor.java:243)
	at org.apache.coyote.AbstractProcessorLight.process(AbstractProcessorLight.java:50)
	at org.apache.coyote.AbstractProtocol$ConnectionHandler.process(AbstractProtocol.java:896)
	at org.apache.tomcat.util.net.NioEndpoint$SocketProcessor.doRun(NioEndpoint.java:1736)
	at org.apache.tomcat.util.net.SocketProcessorBase.run(SocketProcessorBase.java:52)
	at org.apache.tomcat.util.threads.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1191)
	at org.apache.tomcat.util.threads.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:659)
	at org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:63)
	at java.base/java.lang.Thread.run(Thread.java:1583)
Caused by: java.lang.IllegalStateException: In non-blocking mode you may not write to the ServletOutputStream until the previous write has completed and isReady() returns true
	at org.apache.catalina.connector.CoyoteOutputStream.checkNonBlockingWrite(CoyoteOutputStream.java:156)
	at org.apache.catalina.connector.CoyoteOutputStream.write(CoyoteOutputStream.java:101)
	at org.apache.catalina.connector.CoyoteOutputStream.write(CoyoteOutputStream.java:95)
	at org.cometd.server.http.jakarta.JakartaCometDResponse$JakartaCometDOutput.write(JakartaCometDResponse.java:89)
	at org.cometd.server.http.AbstractHttpTransport.writeBegin(AbstractHttpTransport.java:596)
	at org.cometd.server.http.AbstractHttpTransport$Writer.process(AbstractHttpTransport.java:703)
	at org.eclipse.jetty.util.IteratingCallback.processing(IteratingCallback.java:250)
	... 26 more
```