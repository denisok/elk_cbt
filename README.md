# Import cbt results to ELK

Creates ELK cluster and watches for LOGSTASH_DIR directory (see .env file).

So far parses only [cbt](https://github.com/ceph/cbt/) results.

* setup docker, and [docker-compose](https://docs.docker.com/compose/install/)
* check that your system has correct parameters for elk in [docker](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html) `sysctl -w vm.max_map_count=262144`
* clone this repo, `cd elk_cbt`
* `docker-compose up -d`
* name convention for cbt results directory (see pipeline/general.conf) - `branch_tag_cluster_result_desc`:
  * branch - name of the branch (jewel, luminous or product ses5)
  * tag - tag name or milestome name
  * cluster - stable cluster config name - so you could identify what cluster you used for this test
  * result_desc - additional info about test - maybe some additional parameters that aren't tracked in fio results
* create directories *rados* and *librbd* under LOGSTASH_DIR
* copy your cbt results directory to *LOGSTASH_DIR/rados* or to *LOGSTASH_DIR/librbd*
* **be aware** - logstash will delete all imported json files
* go to Kibana interface in web browser http://your_host:5601 (usually localhost)
* don't forget to change Time Filter in the right corner to something more sane (defaults to 15 Min) - maybe to "Year to date"
* use filters, searches and Time Range to filter and look for you data
* `docker-compose down` to stop containers
* elasticsearch data would be stored in docker volume
