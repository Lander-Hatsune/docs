### API request

To create a **Serverless** cluster, send a `POST` request to the `/v1/clusters` endpoint, specifying the following parameters:

- `name`: your cluster's name, a short string with no whitespace.
- `provider`: "GCP" or "AWS".
- `primary_region`: (optional) Specify which region should be made the primary region. This is only applicable to multi-region Serverless clusters. This field is required if you create the cluster in more than one region.
- `regions`: An array of strings specifying the cloud provider's zone codes. For example, for Oregon, set region_name to "us-west2" for GCP and "us-west-2" for AWS.
- `usage_limits`:
  - `request_unit_limit`: (int64) the maximum number of request units that the cluster can consume during the month. If this limit is exceeded, then the cluster is disabled until the limit is increased, or until the beginning of the next month when more free request units are granted. It is an error for this to be zero.
  - `storage_mib_limit`: (int64) the maximum number of Mebibytes of storage that the cluster can have at any time during the month. If this limit is exceeded, then the cluster is throttled; only one SQL connection is allowed at a time, with the expectation that it is used to delete data to reduce storage usage. It is an error for this to be zero.
- `spend_limit`: (integer) the maximum monthly charge for a cluster, in US cents. We recommend using `usage_limits` instead, since `spend_limit` will be deprecated in the future.

[API reference](https://www.cockroachlabs.com/docs/api/cloud/v1.html#post-/api/v1/clusters)

{% include_cached copy-clipboard.html %}
~~~ shell
curl --request POST \
--url 'https://management-staging.crdb.io/api/v1/clusters' \
--header 'Authorization: Bearer { api key }' \
--data @create-serverless-cluster-create.json
~~~

{% include_cached copy-clipboard.html %}
~~~ json
#create-serverless-cluster-create.json
{
  "name": "notorious-moose",
  "provider": "GCP",
  "spec": {
    "serverless": {
      "regions": [
        "us-central1"
      ],
      "usageLimits": 0
    }
  }
}
~~~

### API response

If the request was successful, the API will return information about the newly created cluster.

{% include_cached copy-clipboard.html %}
~~~ json
{
  "id": "89db0bae-89ca-4bb0-83d8-d1e922bcdede",
  "name": "notorious-goose",
  "cockroach_version": "v23.1.4",
  "upgrade_status": "FINALIZED",
  "plan": "SERVERLESS",
  "cloud_provider": "GCP",
  "account_id": "",
  "state": "CREATED",
  "creator_id": "b9ad8253-8c60-46fd-afc2-62fd39e7d2ed",
  "operation_status": "UNSPECIFIED",
  "config": {
    "serverless": {
      "spend_limit": 0,
      "routing_id": "notorious-goose-6132",
      "usage_limits": null
    }
  },
  "regions": [
    {
      "name": "us-central1",
      "sql_dns": "serverless-us-c-1.gcp-us-central1.crdb.io",
      "ui_dns": "",
      "internal_dns": "",
      "node_count": 0,
      "primary": true
    }
  ],
  "created_at": "2023-07-07T01:46:46.862606Z",
  "updated_at": "2023-07-07T01:46:48.901903Z",
  "deleted_at": null,
  "sql_dns": "notorious-goose-6132.6h5c.crdb.io",
  "network_visibility": "PUBLIC",
  "egress_traffic_policy": "UNSPECIFIED"
}
~~~

Where:

  - `{cloud_provider}` is the name of the cloud infrastructure provider on which you want your cluster to run. Possible values are: `GCP`, `AWS`, `AZURE`.
  - `{cluster_id}` is the unique ID of this cluster. Use this ID when making API requests for this particular cluster.
    {{site.data.alerts.callout_info}}
    The cluster ID used in the Cloud API is different than the routing ID used when [connecting to clusters](connect-to-a-serverless-cluster.html).
    {{site.data.alerts.end}}
  - `{cluster_name}` is the name of the cluster you specified when creating the cluster.
  - `{account_id}` is the ID of the account that created the cluster. If the cluster was created using the API, this will be the service account ID associated with the secret key used when creating the cluster.
  - `{region_name}` is the zone code of the cloud infrastructure provider where the cluster is located.
  - `{routing_id}` is the cluster name and tenant ID of the cluster used when [connecting to clusters](connect-to-a-serverless-cluster.html). For example, `funky-skunk-123`.
  - `{server_host}` is the DNS name of the host on which the cluster is located.