# docker-elastiflow

Before running on newly built linux server, please verify virtual memory is configured properly.

This will setup a elastiflow server using the latest version of elasticsearch which is currently 6.5.4. The host machine should have 8GB of RAM minimum. The Xms and Xmx settings can be modified if you want to try it with more or less memory.

"docker-compose up -d" will start it up.

The elastiflow index pattern is inserted automatically by an alpine container that will exit after it is complete. This container waits 60 seconds to give Kibana a chance to start. If kibana is not up before this runs you will notice that on the Management->Index Patterns page elastiflow-* is not listed. If this happens run "docker-compose up -d" again which will restart the alpine container and setup the index.

If the index is available, you can import the dashboards by downloading them from here depending upon version https://github.com/robcowart/elastiflow/tree/master/kibana. This is the direct link suitable for versions 6.4 and 6.5 https://raw.githubusercontent.com/robcowart/elastiflow/master/kibana/elastiflow.dashboards.6.4.x.json.

Import the dashboard by selecting Management->Kibana-Saved Objects->Import->elastiflow.dashboards.6.4.x.json->Import.


# Virtual memory

Elasticsearch uses a mmapfs directory by default to store its indices. The default operating system limits on mmap counts is likely to be too low, which may result in out of memory exceptions.

On Linux, you can increase the limits by running the following command as root:
sysctl -w vm.max_map_count=262144

To set this value permanently, update the vm.max_map_count setting in /etc/sysctl.conf. To verify after rebooting, run sysctl vm.max_map_count.

From <https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html> 

