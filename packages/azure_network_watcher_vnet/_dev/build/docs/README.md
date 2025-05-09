# Azure Network Watcher VNet

[VNet](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview) flow logs in Azure Network Watcher track IP traffic in virtual networks, sending data to Azure Storage for analysis. Unlike NSG flow logs, VNet flow logs offer enhanced monitoring capabilities. They are crucial for understanding network activity, identifying connections, and monitoring open ports. Flow logs serve as the primary source for optimizing resources, ensuring compliance, and detecting intrusions in cloud environments, catering to both startups and enterprises.

## Data streams

This integration supports ingestion of logs from Azure Network Watcher VNet, via [Azure Blob Storage](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-input-azure-blob-storage.html) input.

- **Log** is used to retrieve VNet Flow data. See more details in the documentation [here](https://learn.microsoft.com/en-us/azure/network-watcher/vnet-flow-logs-overview).

## Requirements

Elastic Agent must be installed. For more details and installation instructions, please refer to the [Elastic Agent Installation Guide](https://www.elastic.co/guide/en/fleet/current/elastic-agent-installation.html).

### Installing and managing an Elastic Agent:

There are several options for installing and managing Elastic Agent:

### Install a Fleet-managed Elastic Agent (recommended):

With this approach, you install Elastic Agent and use Fleet in Kibana to define, configure, and manage your agents in a central location. We recommend using Fleet management because it makes the management and upgrade of your agents considerably easier.

### Install Elastic Agent in standalone mode (advanced users):

With this approach, you install Elastic Agent and manually configure the agent locally on the system where it’s installed. You are responsible for managing and upgrading the agents. This approach is reserved for advanced users only.

### Install Elastic Agent in a containerized environment:

You can run Elastic Agent inside a container, either with Fleet Server or standalone. Docker images for all versions of Elastic Agent are available from the Elastic Docker registry, and we provide deployment manifests for running on Kubernetes.

Please note, there are minimum requirements for running Elastic Agent. For more information, refer to the  [Elastic Agent Minimum Requirements](https://www.elastic.co/guide/en/fleet/current/elastic-agent-installation.html#elastic-agent-installation-minimum-requirements).

## Setup

### To collect data from Azure Network Watcher VNet follow the below steps:

1. In the [Azure portal](https://portal.azure.com/), go to your **storage account**.
2. Under **Security + networking**, Click On **Access keys**. Your account access keys appear, as well as the complete connection string for each key.
3. Click On **Show** keys to show your **access keys** and **connection strings** and to enable buttons to copy the values.
4. Under key1, find the Key value. Click On the Copy button to copy the **account key**. Same way you can copy the **storage account name** shown above keys.
5. Go to **Containers** under **Data storage** in your storage account to copy the **container name**.

**Note**:  Enable virtual network flow logs using the steps provided in [reference](https://learn.microsoft.com/en-us/azure/network-watcher/vnet-flow-logs-portal).

### Enabling the integration in Elastic:

1. In Kibana navigate to Management > Integrations.
2. In "Search for integrations" top bar, search for `Azure Network Watcher VNet`.
3. Select the "Azure Network Watcher VNet" integration from the search results.
4. Select "Add Azure Network Watcher VNet" to add the integration.
5. While adding the integration, to collect logs via Azure Blob Storage, keep **Collect VNet logs via Azure Blob Storage** toggle on and then configure following parameters:
   - account name
   - containers
   - service account key/service account uri
6. Save the integration.

## Troubleshooting

The filebeat's [Azure Blob Storage](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-input-azure-blob-storage.html#attrib-expand_event_list_from_field) input can only split events based on a key at root level of JSON. Also the Elasticsearch ingest pipeline [cannot split a message into multiple documents](https://github.com/elastic/elasticsearch/issues/56769). Due to these limitations, the Azure Network Watcher VNet integration cannot split `flowTuples` records, exported via field `azure_network_watcher_vnet.log.records.flows.groups.tuples`, into multiple documents. Each document contains multiple `flowTuples` grouped together. This grouping leads to a loss of direct correlation between fields across a single tuple.

## Logs Reference

### Log

This is the `Log` dataset.

#### Example

{{event "log"}}

{{fields "log"}}
