---
description: >-
  This quick start guide will help you bootstrap a Pinot standalone instance on
  your local machine.
---

# Running Pinot locally

In this guide you'll learn how to download and install Apache Pinot as a standalone instance.

{% hint style="success" %}
This is a quickstart guide that will show you how to quickly start an example recipe in a standalone instance and is meant for learning. To run Pinot in cluster mode, please take a look at [Manual cluster setup](advanced-pinot-setup.md).
{% endhint %}

## Download Apache Pinot

First, let's download the Pinot distribution for this tutorial. You can either build the distribution from source or download a packaged release.

{% hint style="info" %}
**Prerequisites**

Install JDK8 or higher.
{% endhint %}

```bash
# define the pinot version 
PINOT_VERSION=0.3.0
```

### Build from source or download the distribution

{% tabs %}
{% tab title="Build from source " %}
Follow these steps to checkout code from [Github](https://github.com/apache/incubator-pinot) and build Pinot locally

{% hint style="info" %}
**Prerequisites**

Install [Apache Maven](https://maven.apache.org/install.html) 3.6 or higher
{% endhint %}

```bash
# checkout pinot
git clone https://github.com/apache/incubator-pinot.git
cd incubator-pinot

# build pinot
mvn install package -DskipTests -Pbin-dist

# navigate to directory containing the setup scripts
cd pinot-distribution/target/apache-pinot-incubating-$PINOT_VERSION-bin/apache-pinot-incubating-$PINOT_VERSION-bin
```

{% hint style="info" %}
Note that Pinot scripts is located under **pinot-distribution/target** not **target** directory under root.
{% endhint %}
{% endtab %}

{% tab title="Download the release" %}
Download the latest binary release from [Apache Pinot](https://pinot.apache.org/download/), or use this command

```bash
wget https://downloads.apache.org/incubator/pinot/apache-pinot-incubating-$PINOT_VERSION/apache-pinot-incubating-$PINOT_VERSION-bin.tar.gz
```

Once you have the tar file,

```bash
# untar it
tar -zxvf apache-pinot-incubating-$PINOT_VERSION-bin.tar.gz

# navigate to directory containing the launcher scripts
cd apache-pinot-incubating-$PINOT_VERSION-bin
```
{% endtab %}
{% endtabs %}

## Setting up a Pinot cluster

We'll be using a quick-start script, which does the following:

1. Sets up the Pinot cluster `QuickStartCluster`
2. Creates a sample table and loads sample data

There's 3 kinds of quick start

### Batch

Batch quick start creates the pinot cluster, creates an offline table `baseballStats` and pushes sample offline data to the table.

```bash
bin/quick-start-batch.sh
```

That's it! We've spun up a Pinot cluster. You can continue playing with other types of quick start, or simply head on to [Pinot Data Explorer](../features/exploring-pinot.md) to check out the data in the `baseballStats` table.

### Streaming

Streaming quick start sets up a Kafka cluster and pushes sample data to a Kafka topic. Then, it creates the Pinot cluster and creates a realtime table `meetupRSVP` which ingests data from the Kafka topic.

```bash
# stop previous quick start cluster, if any
bin/quick-start-streaming.sh
```

We now have a Pinot cluster with a realtime table! You can head over to [Pinot Data Explorer](../features/exploring-pinot.md) to check out the data in the `meetupRSVP` table.

### Hybrid

Hybrid quick start sets up a Kafka cluster and pushes sample data to a Kafka topic. Then, it creates the Pinot cluster and creates a hybrid table `airlineStats` . The realtime table ingests data from the Kafka topic. Lastly, sample data is pushed into the offline table.

```bash
# stop previous quick start cluster, if any
bin/quick-start-hybrid.sh
```

Let's head over to [Pinot Data Explorer](../features/exploring-pinot.md) to check out the data we pushed to the `airlineStats` table.

