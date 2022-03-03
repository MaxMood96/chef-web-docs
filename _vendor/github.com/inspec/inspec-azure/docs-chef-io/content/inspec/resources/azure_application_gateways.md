+++
title = "azure_application_gateways Resource"
platform = "azure"
draft = false
gh_repo = "inspec-azure"

[menu.inspec]
title = "azure_application_gateways"
identifier = "inspec/resources/azure/azure_application_gateways Resource"
parent = "inspec/resources/azure"
+++

Use the `azure_application_gateways` InSpec audit resource to test properties and configuration of Azure Application Gateways.

## Azure REST API Version, Endpoint, and HTTP Client Parameters

{{% inspec_azure_common_parameters %}}

## Installation

{{% inspec_azure_install %}}

## Syntax

An `azure_application_gateways` resource block returns all Azure Application Gateways, either within a Resource Group (if provided), or within an entire Subscription.
```ruby
describe azure_application_gateways do
  #...
end
```
or
```ruby
describe azure_application_gateways(resource_group: 'my-rg') do
  #...
end
```

## Parameters

`resource_group` _(optional)_

: The name of the resource group.

## Properties

`ids`
: A list of the unique resource ids.

: **Field**: `id`

`locations`
: A list of locations for all the resources being interrogated.

: **Field**: `location`

`names`
: A list of names of all the resources being interrogated.

: **Field**: `name`

`tags`
: A list of `tag:value` pairs defined on the resources being interrogated.

: **Field**: `tags`

`types`
: A list of the types of resources being interrogated.

: **Field**: `type`

`properties`
: A list of properties for all the resources being interrogated.

: **Field**: `properties`

{{% inspec_filter_table %}}

## Examples

**Check Application Gateways are Present.**

```ruby
describe azure_application_gateways do
  it            { should exist }
  its('names')  { should include 'my-appgw' }
end
```
**Filter the Results to Include Only those with Names Match the Given String Value.**

```ruby
describe azure_application_gateways.where{ name.eql?('production-appgw-01') } do
  it { should exist }
end
```

## Matchers

This InSpec audit resource has the following special matchers. For a full list of available matchers, please visit our [Universal Matchers page](https://www.inspec.io/docs/reference/matchers/).

### exists

The control will pass if the filter returns at least one result. Use `should_not` if you expect zero matches.
```ruby
# If we expect 'ExampleGroup' Resource Group to have Application Gateways
describe azure_application_gateways(resource_group: 'ExampleGroup') do
  it { should exist }
end

# If we expect 'EmptyExampleGroup' Resource Group to not have Application Gateways
describe azure_application_gateways(resource_group: 'EmptyExampleGroup') do
  it { should_not exist }
end
```

## Azure Permissions

{{% azure_permissions_service_principal role="contributor" %}}