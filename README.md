# Microsoft-Graph-Calendar-Sync



## Overview

This project demonstrates how to incrementally synchronise Outlook calendar events into a Delta Lake table using the Microsoft Graph API.

The solution leverages the Microsoft Graph `calendarView/delta` endpoint to capture:

* New calendar events
* Updated calendar events
* Deleted calendar events

while maintaining a persistent delta token to avoid expensive full refreshes.

---

## Architecture

```text
Outlook Calendar
       │
       ▼
Microsoft Graph API
(calendarView/delta)
       │
       ▼
Python Notebook
       │
       ▼
Delta Lake
(CalendarEvents)
       │
       ▼
Reporting & Analytics
```

---

## Key Features

### Incremental Synchronisation

Uses Microsoft Graph Delta Queries to retrieve only changes since the previous execution.

### Delta Token Persistence

Stores the Graph delta token in a dedicated table to support true incremental processing.

### Soft Delete Handling

Deleted Outlook events are preserved and marked as:

```text
is_deleted = true
```

allowing historical analysis while reflecting current calendar state.

### Retry Logic

Handles:

* API throttling (429)
* Temporary service errors (502, 503, 504)

using retry and backoff strategies.

### Delta Lake Merge

Performs:

* Inserts
* Updates
* Soft Deletes

using Delta Lake merge operations.

---

## Technology Stack

* Microsoft Graph API
* Python
* Apache Spark
* Delta Lake
* Microsoft Fabric
* Lakehouse Architecture

---

## Data Flow

1. Authenticate with Microsoft Graph
2. Retrieve existing delta token
3. Request new and changed events
4. Capture deleted events
5. Flatten Graph API JSON responses
6. Merge into Delta Lake
7. Persist latest delta token
8. Generate sync summary

---

## Example Use Cases

* Meeting room utilisation reporting
* Resource scheduling analytics
* Calendar activity monitoring
* Operational reporting
* Outlook calendar ingestion into a Lakehouse

---

## Security

This repository contains no production credentials.

Replace the following placeholders before execution:

```python
TENANT_ID
CLIENT_ID
CLIENT_SECRET
SHARED_MAILBOX
```

Store secrets securely using your preferred secret management solution.

---

## Future Enhancements

* Multi-calendar support
* Event attendee analytics
* Room booking analytics
* Power BI reporting layer
* Near real-time sync orchestration

---

## Author

Ishfaq Malik

Databricks Certified Data Engineer Professional
Databricks Certified Data Engineer Associate
