---
title: "Notification functions | Snowflake Documentation"
source: https://docs.snowflake.com/sql-reference/functions-notification
cert_domain: domain-2-data-preparation-and-feature-engineering
crawl_depth: 1
crawled: 2026-08-23
---

# Notification functions¶

Notification functions are helper functions that you can call when using the [SYSTEM$SEND_SNOWFLAKE_NOTIFICATION](/sql-reference/stored-procedures/system_send_snowflake_notification) stored procedure to [send a notification](/user-guide/notifications/snowflake-notifications).

The integration configuration and message construction functions return JSON-formatted strings that you pass to the SYSTEM$SEND_SNOWFLAKE_NOTIFICATION stored procedure.

Sub-category| Function| Notes  
---|---|---  
Integration Configuration| [EMAIL_INTEGRATION_CONFIG](/sql-reference/functions/email_integration_config)|   
| [INTEGRATION](/sql-reference/functions/integration)|   
Message Construction| [APPLICATION_JSON](/sql-reference/functions/application_json)|   
| [TEXT_HTML](/sql-reference/functions/text_html)|   
| [TEXT_PLAIN](/sql-reference/functions/text_plain)|   
Message Sanitization| [SANITIZE_WEBHOOK_CONTENT](/sql-reference/functions/sanitize_webhook_content)|
