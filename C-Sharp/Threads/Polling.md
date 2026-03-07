#SoftDev 
**Polling** is method used in system design to check the status or gather data from multiple sources periodically. It involves continuously querying or checking devices, or other components at predetermined intervals to see if there's any new info or if certain conditions have been met.

- In the context of system designing, polling typically involves a **central system** (like a Server) regularly sending requests to multiple client devices or nodes to gather info or to determine their status.
- This can include checking for updates, retrieving data, or verifying if certain tasks have been completed.

**Regular Polling**: simplest form of polling, where the controlling entity queries the source at regular intervals. The interval can be fixed or dynamically adjusted based on factors such as system load or the urgency of data updates.

**Adaptive Polling**: polling interval is dynamically adjusted based on factors such as the rate of change of data, the responsiveness of the source etc.

**Priority-Based Polling**: In systems where multiple sources need to be polled, priority-based polling ensures that $>$ critical sources are queried $>$ frequently than < critical ones. This prioritization helps ensure that resources are allocated efficiently based on the importance of the data.

**Grouped Polling**: grouping similar sources together & polling them collectively rather than individually. This reduces overhead by minimizing the num of polling requests sent & responses received, especially useful when dealing with a large num of sources.

**Event-Driven Polling**: controlling entity triggers  & ling request in response to specific events or conditions, rather than at regular intervals. This approach helps reduce unnecessary polling & can improve responsiveness in systems where data updates are sporadic.

**Async Polling**: controlling entity initiates polling requests async, allowing it to continue exec other tasks while waiting for responses.