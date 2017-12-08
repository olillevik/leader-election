# DYI

## Leader election
Leader election for distributed work can be done by synchronzing them on the database.

```sql
# SQLLite specific query
CREATE TABLE elected_leader (
    id TEXT,
    last_seen_active INT
)

# Election query
UPDATE elected_leader SET id='your_node_id', last_seen_active=strftime('%s', 'now') 
WHERE last_seen_active<strftime('%s', 'now')-30 OR id='your_node_id';
```

```sql
# Oracle specific query
CREATE TABLE elected_leader (
    id VARCHAR2(100),
    last_seen_active TIMESTAMP
)

# Election query
UPDATE elected_leader SET id = 'your_node_id', last_seen_active = CURRENT_TIMESTAMP 
WHERE last_seen_active < CURRENT_TIMESTAMP - INTERVAL '60' SECOND OR elected_leader = 'your_node_id';
```
Run the query with shorter intervals than the set timeout (30 seconds in this example).
You are elected if this SQL returns 1 and slave if it returns 0

Next, whenever you do work you need to ensure you are still the leader when performing the task. This is the hardest part and is left as an exersice to the reader. Remember: Latency in distributed systems is NOT your friend.
