# Controller

The Pinot Controller is responsible for a number of things

* Controllers maintain the **global metadata** \(e.g. configs and schemas\) of the system with the help of Zookeeper which is used as the  persistent metadata store.
* Controllers host **Helix Controller** and is responsible for managing other pinot components \(brokers, servers, minions\) 
* They maintain the **mapping of which servers are responsible for which segments**. This mapping is used by the servers, to download the portion of the segments that they are responsible for. This mapping is also used by the broker to decide which servers to route the queries to.
* Controller has **admin endpoints** for viewing, creating, updating and deleting configs which help us manage and operate the cluster.
* Controllers also have endpoints for **segment uploads** which are used in offline data pushes. They are responsible for initializing **realtime consumption** and coordination of persisting the realtime segments into the segment store periodically.
* They undertake other **management activities** such as managing retention of segments, validations.

There can be multiple instances of Pinot controller for redundancy. If there are multiple controllers, Pinot expects that all of them are configured with the same back-end storage system so that they have a common view of the segments \(_e.g._ NFS\). Pinot can use other storage systems such as HDFS or [ADLS](https://azure.microsoft.com/en-us/services/storage/data-lake-storage/).

## Starting a Controller

Make sure you've [setup Zookeeper](cluster.md#setup-a-pinot-cluster). If you're using docker, make sure to [pull the pinot docker image](cluster.md#setup-a-pinot-cluster). To start a controller

{% tabs %}
{% tab title="Docker Image" %}
```text
docker run \
    --network=pinot-demo \
    --name pinot-controller \
    -p 9000:9000 \
    -d ${PINOT_IMAGE} StartController \
    -zkAddress pinot-zookeeper:2181
```
{% endtab %}

{% tab title="Launcher Scripts" %}
```text
bin/pinot-admin.sh StartController \
  -zkAddress localhost:2181 \
  -clusterName PinotCluster \
  -controllerPort 9000
```
{% endtab %}
{% endtabs %}

