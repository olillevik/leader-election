# master-slave


CREATE TABLE elected_leader (
    id text,
    last_seen_active int
)

#You are master if this SQL returns 1 and slave if it returns 0
UPDATE elected_leader SET id='your_node_id', last_seen_active=strftime('%s', 'now') WHERE last_seen_active<strftime('%s', 'now')-30 or id='your_node_id';
