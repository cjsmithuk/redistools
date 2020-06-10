# Redis "oh shit" tools

redis-cli wrapper scripts that allow you to get out of sticky situations like
minor ETL scenarios. Use after uttering "oh shit".

## Usage

1. Get a list of keys that you want to export in file `exportkeys`
2. Dump keys: `cat exportkeys | ./dumpkeys > keysdumped`
3. This will dump out space delimited key-value pairs, one per line of key + base64 encoded binary value.
4. Restore keys: `cat keysdumped | ./restorekeys`

Individual tools `dumpkey` and `restorekey` are available for single keys.

## Typical usage scenarios

### To restore deleted keys in a replicated cluster

1. copy the AOF file from the production server to a secure scratch node.
2. Find the DEL entry for the key(s) you need back. 
3. Delete everything in the AOF file from just before that point to the end.
4. Fire this AOF file up in a separate redis instance.
5. Use `dumpkeys` to extract the keys you need from the scratch node.
6. Use `restorekeys` to restore them onto the production node.
7. Leave and go to pub.
