# elasticsearch-archive-restore


## elasticsearch has below functions:
* Take backup of indexes in snapshot form
* Archive old indexes
* Delete old snapshots
* Restore old snapshots 

## How to execute curator script
```sh
$ elasticsearch-archive-restore archiveindex - This function create backup of indices and delete indices older than than user defined months
$ elasticsearch-archive-restore deletesnapshots - This function deletes snapshots older than user defined months
$ elasticsearch-archive-restore restoresnapshot - This function display all existing snapshots and restore elasticsearch to selected snapshot
```
## Main commands and usage of curator-elasticsearch

### Register a repo (esbackup) with elasticsearch 
```sh
$curl -XPUT http://$ADDRESS/_snapshot/reponame -H 'Content-Type: application/json' -d '{ "type": "fs", "settings": { "location": "/usr/share/elasticsearch/archive", "compress": true}}'
```
### Take backup of indices
```sh
curl -XPUT -s http://$ADDRESS/_snapshot/$reponame/$CURRENTDATE?wait_for_completion=true -H 'Content-Type: application/json' -d '{ "ignore_unavailable": true, "include_global_state": false }'
```
### Display all indices
```sh
curl -s http://$ADDRESS/_cat/indices?h=i,cd
```
### Delete indices
```sh
curl -XDELETE http://$ADDRESS/$indexname
```
### Delete snapshot
```sh
curl -XDELETE http://$ADDRESS/_snapshot/$REPONAME/$snapshotname
```
### Display existing snapshots
```sh
curl -X GET "$ADDRESS/_cat/snapshots/$REPONAME?v&s=id" | sed -n '1!p' 
```
### Restore snapshot
```sh
curl -XPOST "$ADDRESS/_snapshot/$REPONAME/$snapshotname/_restore?wait_for_completion=true"
```
