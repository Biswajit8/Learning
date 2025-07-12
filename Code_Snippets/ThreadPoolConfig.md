```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor;
import org.springframework.beans.factory.annotation.Value;

import lombok.extern.slf4j.Slf4j;

@Configuration
@Slf4j
public class ThreadPoolConfig {

    @Value("${thread.pool.core-size:10}")
    private int corePoolSize;

    @Value("${thread.pool.max-size:20}")
    private int maxPoolSize;

    @Value("${thread.pool.queue-capacity:50}")
    private int queueCapacity;

    @Value("${thread.pool.keep-alive-seconds:60}")
    private int keepAliveSeconds;

    @Value("${thread.pool.thread-name-prefix:task-executor-}")
    private String threadNamePrefix;

    @Bean("taskExecutor")
    public ThreadPoolTaskExecutor taskExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(corePoolSize);
        executor.setMaxPoolSize(maxPoolSize);
        executor.setQueueCapacity(queueCapacity);
        executor.setKeepAliveSeconds(keepAliveSeconds);
        executor.setThreadNamePrefix(threadNamePrefix);
        executor.setWaitForTasksToCompleteOnShutdown(true);
        executor.setAwaitTerminationSeconds(60);
        executor.initialize();
        
        log.info("ThreadPoolTaskExecutor initialized with corePoolSize={}, maxPoolSize={}, queueCapacity={}, keepAliveSeconds={}, threadNamePrefix={}",
                corePoolSize, maxPoolSize, queueCapacity, keepAliveSeconds, threadNamePrefix);
        
        return executor;
    }
}
```
