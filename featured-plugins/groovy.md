---
description: >-
  Enhance the developer experience by scripting the Live API and make proof of
  concepts even faster
---

# Groovy support

The groovy plugin is available for download on [Intelie Live Marketplace](https://marketplace.intelie.com/artifact/plugin-groovy).

It provides the capability to register and run Groovy snippets, using the same API that is available to Java.

![Snippets Groovy is available under Platform Customization menu in Administration page](<../.gitbook/assets/image (118).png>)

A Groovy snippet is limited to be a standalone piece of code, which means it is unable to declare and use other plugin dependencies. If you need to write a more advanced Groovy code, you must upload that code as a [Groovy script in Plugins administration page](../developers/plugins.md#groovy-scripts).

For example, the snippet below adds a resource to get a thread dump.

```java
import java.lang.management.LockInfo;
import java.lang.management.ManagementFactory;
import java.lang.management.MonitorInfo;
import java.lang.management.ThreadInfo;

import javax.ws.rs.GET;
import javax.ws.rs.Path;
import javax.ws.rs.Produces;

live.web().addService("threaddump", new ThreadDumpService());

@Path("/")
@Produces("text/plain")
public class ThreadDumpService {

    @GET
    public String threadDump() {
        return formatStackTraces() + 
          "\n---------------------------------------------------\n" +
          memoryInformation() +
           "\n---------------------------------------------------\n" +
          garbageCollectorInformation()
    }
  
    String formatStackTraces() {
        ThreadInfo[] allThreads = ManagementFactory.getThreadMXBean().dumpAllThreads(true, true);

        StringBuilder dump = new StringBuilder();

        for (ThreadInfo threadInfo : allThreads) {
            dump.append("Thread: [").append(threadInfo.getThreadName())
                    .append("] state=").append(threadInfo.getThreadState()).append("\n");

            if (threadInfo.getLockInfo() != null) {
                dump.append("Waiting ").append(threadInfo.getLockInfo()).append("\n");
            }

            if (threadInfo.getLockedMonitors() != null && threadInfo.getLockedMonitors().length > 0) {
                dump.append("Currently locking monitors:\n");
                for(MonitorInfo monitor : threadInfo.getLockedMonitors()) {
                    dump.append(" - ").append(monitor).append("\n");
                }
            }

            if (threadInfo.getLockedSynchronizers() != null && threadInfo.getLockedSynchronizers().length > 0) {
                dump.append("Currently locking Synchronizers:\n");
                for(LockInfo synchronizer : threadInfo.getLockedSynchronizers()) {
                    dump.append(" - ").append(synchronizer).append("\n");
                }
            }

            for (StackTraceElement traceElement : threadInfo.getStackTrace()) {
                dump.append("    at ").append(traceElement).append("\n");
            }

            dump.append("\n");
        }

        return dump.toString();
    }
 
  String memoryInformation() {
    StringBuilder memInfo = new StringBuilder();
    
    def memMxBean = ManagementFactory.memoryMXBean
    
    memInfo.append("Head Memory Usage: ").append(memMxBean.heapMemoryUsage)
    	.append("\nNonHead Memory Usage: ").append(memMxBean.nonHeapMemoryUsage)
    
    return memInfo.toString();
  }
  
  String garbageCollectorInformation() {
    
    StringBuilder gcInfo = new StringBuilder();
    
    ManagementFactory.garbageCollectorMXBeans.each {gc ->
      gcInfo.append("MemoryManager: ").append(gc.name).append(" valid: ").append(gc.valid)
      	.append(" collectionCount: ").append(gc.collectionCount).append(" collectionTime: ").append(gc.collectionTime).append("\n")
    }
    
    return gcInfo.toString()
  }
}

```
