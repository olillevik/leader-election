# Leader election
Leader election for distributed work can be done by synchronzing them on the database.

```sql
CREATE TABLE elected_leader (
    id TEXT,
    last_seen_active INT
)

# Run this with reasonably shorter intervals than the set timeout (30 seconds in this example).
# You are elected if this SQL returns 1 and slave if it returns 0
UPDATE elected_leader SET id='your_node_id', last_seen_active=strftime('%s', 'now') 
WHERE last_seen_active<strftime('%s', 'now')-30 OR id='your_node_id';
```

## Caveat
Being elected leader at some point in time doesn't guarantee the same component being the leader when doing work later on. Latency in distributed systems is NOT your friend.
