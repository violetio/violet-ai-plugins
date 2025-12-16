# Temporal Workflow Patterns

## Configuration
All Temporal workflows and activities MUST use constants from `TemporalConfig.java`:

```java
public class TemporalConfig {
    public static final String WORKFLOW_QUEUE = "workflows";
    public static final String ACTIVITY_QUEUE = "activities";
}
```

## Task Queue Usage
**NEVER use hardcoded task queue strings:**

```java
// WRONG - No workers on this queue
WorkflowOptions.newBuilder()
    .setTaskQueue("CUSTOM_QUEUE")
    .build();

// CORRECT - Workers polling this queue
WorkflowOptions.newBuilder()
    .setTaskQueue(TemporalConfig.WORKFLOW_QUEUE)
    .build();
```

## Activity Registration
**Activities MUST have `@ActivityImpl` annotation:**

```java
// CORRECT - Activity registered
@Component
@ActivityImpl(taskQueues = TemporalConfig.ACTIVITY_QUEUE)
public class MyActivitiesImpl implements MyActivities { }

// WRONG - Activity NOT registered
@Component
public class MyActivitiesImpl implements MyActivities { }
```

## Symptoms of Task Queue Mismatch
1. Temporal UI: "There are no Workers polling the [QUEUE_NAME] Task Queue"
2. Workflows never start execution
3. Status queries timeout after 60+ seconds
4. Workflow appears in Temporal but stays "Running" forever
5. Error: `Activity Type "MyActivityMethod" is not registered with a worker`

## gRPC Keep-Alive Configuration
Prevents connection timeout during long-running workflows:

```properties
spring.temporal.connection.enable-keep-alive=true
spring.temporal.connection.keep-alive-time=30s
spring.temporal.connection.keep-alive-timeout=15s
spring.temporal.connection.keep-alive-permit-without-stream=true
```

## Worker Tuning (for bulk operations)
```properties
spring.temporal.workflow-cache.max-instances=100
spring.temporal.workflow-cache.max-threads=50
spring.temporal.workers.max-concurrent-workflow-task-pollers=5
spring.temporal.workers.max-concurrent-activity-task-pollers=5
```

## Workflow Interface Pattern
```java
@WorkflowInterface
public interface MyWorkflow {
    @WorkflowMethod
    void execute(WorkflowInput input);

    @QueryMethod
    WorkflowState getState();

    @SignalMethod
    void updateState(StateUpdate update);
}
```

## Activity Interface Pattern
```java
@ActivityInterface
public interface MyActivities {
    @ActivityMethod
    ActivityResult performAction(ActivityInput input);
}
```

## Error Handling
- Use `ApplicationFailure` for business errors
- Use `TemporalFailure` for infrastructure errors
- Configure retry policies per activity type
