---
title: "EXECUTE TASK | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/sql/execute-task
cert_domain: domain-3-model-development
crawl_depth: 1
crawled: 2026-08-23
---

# EXECUTE TASK¶

Manually triggers an asynchronous single run of a task (either a standalone task or the root task in a [task graph](/user-guide/tasks-graphs#label-task-dag)) independent of the schedule defined for the task.

A successful run of a root task triggers a cascading run of child tasks in the task graph as their precedent task completes, as though the root task had run on its defined schedule.

Additionally, you can manually trigger the re-execution of a previously failed task.

See also:
    

[CREATE TASK](/sql-reference/sql/create-task) , [DESCRIBE TASK](/sql-reference/sql/desc-task) , [ALTER TASK](/sql-reference/sql/alter-task) , [DROP TASK](/sql-reference/sql/drop-task) , [SHOW TASKS](/sql-reference/sql/show-tasks)

## Syntax¶
[code] 
    EXECUTE TASK <name>
      [ USING CONFIG = <configuration_string> ]
    
    EXECUTE TASK <name> RETRY LAST
    
    EXECUTE TASK <name> RETRY GRAPH RUN GROUP '<graph_run_group_id>'
    
[/code]

## Parameters¶

`_name_`
    

Identifier for the standalone task or root task to run. If the identifier contains spaces or special characters, the entire string must be enclosed in double quotes. Identifiers enclosed in double quotes are also case-sensitive.

`USING CONFIG = _configuration_string_`
    

Specifies a configuration string in valid JSON format for this single execution. This parameter creates a new execution with the dynamically specified configuration, but doesn’t modify the task definition.

Snowflake merges the dynamic configuration with the _default_ configuration, which is the CONFIG parameter that you set in the task definition with CREATE or ALTER. For matching fields, Snowflake uses the dynamically specified values. For non-matching fields, Snowflake uses the values from the default configuration. For an example, see Use a dynamic CONFIG.

The configuration string follows the same format as the CONFIG parameter in [CREATE TASK](/sql-reference/sql/create-task) or [ALTER TASK](/sql-reference/sql/alter-task):
[code]
    CONFIG = $${"string1": value1 [, "string2": value2, ...] }$$
    
[/code]

Example:
[code]
    CONFIG = $${"learning_rate": 0.2, "environment": "testing"}$$
    
[/code]

`RETRY LAST`
    

Re-execute the last failed task of the [task graph](/user-guide/tasks-graphs#label-task-dag) with `_name_` restarting from where the tasks failed.

To re-execute a task the following conditions must be met:

  * The last task graph run must be in state FAILED or CANCELED.
  * The task graph must not have been modified since it was last run.
  * The last failed graph run’s first attempt must have been executed in the last 14 days.



To view task history, see either the [TASK_HISTORY](/sql-reference/functions/task_history) table function or the [Tasks page on Snowsight](/user-guide/ui-snowsight-tasks).

Note

RETRY LAST creates a new graph run which begins execution at the last failed task(s).

Specifically, all FAILED or CANCELED task runs are immediately re-executed, and associated child tasks are scheduled if all of their predecessors execute successfully.

Additionally the new task graph run produced by the retry will have an ATTEMPT NUMBER that is one greater than the previous failed graph run, and share the same GRAPH_RUN_GROUP_ID as the retried, or original task graph run.

`RETRY GRAPH RUN GROUP _'graph_run_group_id'_`
    

Re-execute starting from the failed tasks of a specific previous graph run, identified by its GRAPH_RUN_GROUP_ID. Use this variant when you want to retry a specific graph run rather than the most recent one. `RETRY LAST` always targets the latest run.

To re-execute a graph run the following conditions must be met:

  * The latest run of the specified graph run group must be complete (it must not have a run still in progress) and must have at least one task in state FAILED or CANCELED.
  * The specified graph run group must belong to the task identified by `_name_`.
  * The task graph must not have been modified since the targeted run.
  * The targeted graph run group’s first attempt must have been executed in the last 14 days.



To find a graph run’s GRAPH_RUN_GROUP_ID, query the [TASK_HISTORY](/sql-reference/functions/task_history) or [COMPLETE_TASK_GRAPHS](/sql-reference/functions/complete_task_graphs) table function, or use the [Tasks page on Snowsight](/user-guide/ui-snowsight-tasks).

Note

Like `RETRY LAST`, this command creates a new graph run that begins execution at the failed task(s) of the targeted graph run. Specifically, all FAILED or CANCELED task runs in the targeted graph run are immediately re-executed, and associated child tasks are scheduled if all of their predecessors execute successfully.

The new task graph run produced by the retry will have an ATTEMPT_NUMBER that is one greater than the targeted graph run, and shares the same GRAPH_RUN_GROUP_ID as the targeted graph run.

## Usage notes¶

  * Executing a task requires either the OWNERSHIP or OPERATE privilege on the task.

When the EXECUTE TASK command triggers a task run, Snowflake verifies that the role with the OWNERSHIP privilege on the task also has the USAGE privilege on the warehouse assigned to the task, as well as the global EXECUTE TASK privilege; if not, an error is produced.

Tasks always run with the privileges of the original owner role, even if a different role with the OPERATE privilege uses EXECUTE TASK to run the task.

  * By default, Snowflake runs tasks by using the system user with the privileges of the task owner role. To run a task as a specific user, configure the task with EXECUTE AS USER. For more information, see [Run tasks with user privileges](/user-guide/tasks-intro#label-user-based-security-for-tasks).

  * For the USING CONFIG option:

    * If the task graph is currently executing and you run this command, Snowflake waits for the current execution to complete before starting a new execution with the dynamic configuration.
    * If you run this command multiple times while a task is executing, Snowflake uses the configuration from the most recent command for the next run. Previous configurations are replaced and won’t be executed.
    * The dynamic configuration only applies to the single execution triggered by this command. Subsequent scheduled runs use the default CONFIG parameter from the task definition.
  * The SQL command can only execute a standalone task or the root task in a task graph. If a child task is input, the command returns a user error.

  * Manually executing a standalone or root task establishes a version of the task. The standalone task or entire task graph completes its run with this version. For more information about task versions, see [Versioning of task runs](/user-guide/tasks-intro#label-versioning-of-task-runs).

  * A suspended root task is run without resuming the task; there is no need to explicitly resume the root task before you execute this SQL command. However, EXECUTE TASK does not automatically resume child tasks in the task graph. The command skips any child tasks that are suspended.

To recursively resume all dependent tasks tied to a root task in a task graph, query the [SYSTEM$TASK_DEPENDENTS_ENABLE](/sql-reference/functions/system_task_dependents_enable) function rather than enabling each task individually (using ALTER TASK … RESUME).

As a best practice when testing new or modified task graphs, set the root task to run on its intended production schedule but leave it in a suspended state. When you have tested the task graph successfully, resume the root task. Note that you must resume any suspended child tasks in the task graph for testing; otherwise, they are skipped during runs of the task graph.

  * If no instance of the task is running, a new run starts immediately.

  * If another instance is scheduled (that is, if the task shows a SCHEDULED state in the [TASK_HISTORY](/sql-reference/functions/task_history) output), the requested run replaces the scheduled run. The requested run starts immediately, using the current timestamp as the scheduled time.

  * If the task or task graph is currently queueing or executing (that is, if the task shows an EXECUTING state in the [TASK_HISTORY](/sql-reference/functions/task_history) output), then the current run continues using the [task version](/user-guide/tasks-intro#label-versioning-of-task-runs) that was current when the command was executed. A new run is then scheduled to start, at a time depending on the task type:

    * For standalone tasks, a new run is scheduled to start after the current run completes.
    * For task graphs, the behavior depends on the OVERLAP_POLICY setting. For more information, see [OVERLAP_POLICY](/sql-reference/sql/create-task#label-create-task-optional-parameters) in the CREATE TASK documentation.

If the EXECUTE TASK command is executed again before the next scheduled run starts, the requested run replaces the scheduled run.

  * If a task fails with an unexpected error, you can receive a notification about the error. For more information on configuring task error notifications refer to [Set up error notifications for tasks](/user-guide/tasks-errors).

  * To view the task information you can either:

    * In Snowsight, in the navigation menu, select **Transformation** » **Tasks**.
    * Call the [COMPLETE_TASK_GRAPHS](/sql-reference/functions/complete_task_graphs) table function, and examine the results.



## Examples¶

The following examples show how to manually trigger a task run, how to retry a specific previous graph run, and how to use a dynamic CONFIG.

### Manually trigger a task run¶

Manually trigger a run of a task named `mytask`:
[code] 
    EXECUTE TASK mytask;
    
[/code]

### Retry a specific graph run¶

Retry the failed tasks of a specific previous graph run of `mytask`, identified by its GRAPH_RUN_GROUP_ID:
[code] 
    EXECUTE TASK mytask RETRY GRAPH RUN GROUP '33af30ab-f960-4ba0-a5c8-d132e5623468';
    
[/code]

To find the GRAPH_RUN_GROUP_ID for a previous run, query the [TASK_HISTORY](/sql-reference/functions/task_history) or [COMPLETE_TASK_GRAPHS](/sql-reference/functions/complete_task_graphs) table function.

### Use a dynamic CONFIG¶

Create a root task named `my_root_task` with a default configuration:
[code] 
    CREATE OR REPLACE TASK my_root_task
      WAREHOUSE = regress
      SCHEDULE = '10 m'
      CONFIG = $${
        "environment": "production",
        "output_paths": {
          "logs": "/prod/logs",
          "results": "/prod/results"
        }
      }$$
      AS ...;
    
[/code]

Now, execute the task and specify a dynamic configuration:
[code] 
    EXECUTE TASK my_root_task
      USING CONFIG=$${
        "output_paths": {
          "results": "/temp/testing"
        }
      }$$;
    
[/code]

The following example shows the resulting configuration for this execution:
[code] 
    {
      "environment": "production",
      "output_paths": {
        "logs": "/prod/logs",
        "results": "/temp/testing"
      }
    }
    
[/code]

The `environment` field and the `output_paths.logs` field remain unchanged from the default configuration; only `output_paths.results` is updated with the dynamic value.
