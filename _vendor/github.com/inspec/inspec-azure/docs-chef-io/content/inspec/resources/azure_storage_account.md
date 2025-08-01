+++
title = "azure_storage_account Resource"
platform = "azure"
draft = false
gh_repo = "inspec-azure"

[menu.inspec]
title = "azure_storage_account"
identifier = "inspec/resources/azure/azure_storage_account Resource"
parent = "inspec/resources/azure"
+++

Use the `azure_storage_account` InSpec audit resource to test the properties related to an Azure Storage account.

## Azure REST API Version, Endpoint, and HTTP Client Parameters

{{< readfile file="content/inspec/resources/reusable/md/inspec_azure_common_parameters.md" >}}

## Install

{{< readfile file="content/inspec/resources/reusable/md/inspec_azure_install.md" >}}

## Syntax

An `azure_storage_account` resource block identifies an Azure storage account by `name` and `resource_group`, or the `resource_id`.

```ruby
describe azure_storage_account(resource_group: 'RESOURCE_GROUP', name: 'NAME')  do
  it { should exist }
end
```

```ruby
describe azure_storage_account(resource_id: '/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{accountName}') do
  it { should exist }
end
```

## Parameters

`resource_group`
: Azure resource group where the targeted resource resides.

`name`
: The name of the storage account within the specified resource group.

`resource_id`
: The unique resource ID.

`activity_log_alert_api_version`
: The activity log alerts endpoint API version used in the `have_recently_generated_access_key` matcher. The latest version will be used unless provided.

`storage_service_endpoint_api_version`
: The storage service endpoint API version. `2019-12-12` will be used unless provided.

`resource_data`
: In-memory cached Azure Network security group data. Passing data to this parameter can increase performance since it avoids multiple network calls to the same Azure resource. When provided, it binds the values directly to the resource. Data passed to the `resource_data` parameter could be stale. It is the user's responsibility to refresh the data.

Pass one of the following parameter sets for a valid query:

- `resource_id`
- `resource_group` and `name`
- `resource_data`

## Properties

`queues<superscript>*</superscript>`
: Lists all of the queues in a given storage account. For more information, see the [Azure Storage Services List Queues documentation](https://docs.microsoft.com/en-us/rest/api/storageservices/list-queues1).

`queue_properties<superscript>*</superscript>`
: Gets the properties of a storage account's Queue service, including properties for Storage Analytics and CORS (Cross-Origin Resource Sharing) rules. For more information, see the [Azure Queue Service Properties documentation](https://docs.microsoft.com/en-us/rest/api/storageservices/get-queue-service-properties).

`blobs<superscript>*</superscript>`
: Lists all of the blob containers in a given storage account. For more information, see the [Azure Storage Services List Containers](https://docs.microsoft.com/en-us/rest/api/storageservices/list-containers2).

`blob_properties<superscript>*</superscript>`
: Gets the properties of a storage account's Blob service, including properties for Storage Analytics and CORS (Cross-Origin Resource Sharing) rules. For more information, see the [Azure Storage Services Blob Service Properties documentation](https://docs.microsoft.com/en-us/rest/api/storageservices/get-blob-service-properties).

`table_properties<superscript>*</superscript>`
: Gets the properties of a storage account's Table service, including properties for Storage Analytics and CORS (Cross-Origin Resource Sharing) rules. For more information, see the [Azure Storage Services Table Service Properties documentation](https://docs.microsoft.com/en-us/rest/api/storageservices/get-table-service-properties).

<superscript>*</superscript>: These Azure endpoints return data in XML format. However, they're converted to make the properties accessible with dot notation.

The property names are in snake case, `property_name`. Therefore, you can test `<EnumerationResults ServiceEndpoint="https://myaccount.queue.core.windows.net/">` with `its('enumeration_results.service_endpoint)`.

For properties applicable to all resources, such as `type`, `name`, `id`, and `properties`, refer to [`azure_generic_resource`]({{< relref "azure_generic_resource.md#properties" >}}).

Also, see the [Azure documentation](https://docs.microsoft.com/en-us/rest/api/storagerp/storageaccounts/getproperties#storageaccount) for other available properties. You can access any attribute in the response with the key names separated by dots (`.`).

## Examples

### Test the primary endpoints

```ruby
describe azure_storage_account(resource_group: 'RESOURCE_GROUP', name: 'NAME') do
  its('properties.primaryEndpoints.blob') { should cmp 'https://mysa.blob.core.windows.net/' }
  its('properties.primaryEndpoints.queue') { should cmp 'https://mysa.queue.core.windows.net/' }
  its('properties.primaryEndpoints.table') { should cmp 'https://mysa.table.core.windows.net/' }
  its('properties.primaryEndpoints.file') { should cmp 'https://mysa.file.core.windows.net/' }
end
```

### Verify that only HTTPS is supported

```ruby
describe azure_storage_account(resource_group: 'RESOURCE_GROUP', name: 'NAME') do
  its('properties.supportsHttpsTrafficOnly') { should be true }
end
```

### Test queues service endpoint

```ruby
describe azure_storage_account(resource_group: 'RESOURCE_GROUP', name: 'NAME') do
  its('queues.enumeration_results.service_endpoint') { should cmp 'https://mysa.queue.core.windows.net/' }
end
```

### Test Blobs service endpoint

```ruby
describe azure_storage_account(resource_group: 'RESOURCE_GROUP', name: 'NAME') do
  its('blobs.enumeration_results.service_endpoint') { should cmp 'https://mysa.blob.core.windows.net/' }
end
```

### Test queue properties logging version

```ruby
describe azure_storage_account(resource_group: 'RESOURCE_GROUP', name: 'NAME') do
  its('queue_properties.logging.version') { should cmp '1.0' }
end
```

### Test Blob properties logging version

```ruby
describe azure_storage_account(resource_group: 'RESOURCE_GROUP', name: 'NAME') do
  its('blob_properties.logging.version') { should cmp '1.0' }
end
```

### Test table properties logging version

```ruby
describe azure_storage_account(resource_group: 'RESOURCE_GROUP', name: 'NAME') do
  its('table_properties.logging.version') { should cmp '1.0' }
end
```

### Loop through all storage accounts and test a blob service endpoint exists from already cached data

```ruby
azure_storage_accounts.entries.each do |azure_storage_account_data|
    describe azure_storage_account(resource_data: azure_storage_account_data) do
      its('blobs.enumeration_results.service_endpoint') { should cmp 'https://mysa.blob.core.windows.net/' }
    end
end
```

## Matchers

This InSpec audit resource has the following special matchers. For a full list of available matchers, please visit our [Universal Matchers page](https://docs.chef.io/inspec/matchers/).

### have_encryption_enabled

Test if encryption is enabled.

```ruby
describe azure_storage_account(resource_group: 'RESOURCE_GROUP', name: 'NAME') do
  it { should have_encryption_enabled }
end
```

### have_recently_generated_access_key

Test if an access key has been generated within the last **90** days.

```ruby
describe azure_storage_account(resource_group: 'RESOURCE_GROUP', name: 'NAME') do
  it { should have_recently_generated_access_key }
end
```

### exists

```ruby
# If we expect the resource to always exist.

describe azure_storage_account(resource_group: 'RESOURCE_GROUP', name: 'NAME') do
  it { should exist }
end
```

### not_exists

```ruby
# If we expect the resource to never exist.

describe azure_storage_account(resource_group: 'RESOURCE_GROUP', name: 'NAME') do
  it { should_not exist }
end
```

## Azure Permissions

{{% inspec-azure/azure_permissions_service_principal role="reader" %}}
