+++
title = "UsePlatformHelpers"

+++

<!-- This content is automatically generated. See https://github.com/chef/chef-web-docs/blob/main/generated/README.md -->

The department is: `UsePlatformHelpers`

The full name of the cop is: `Chef/Style/UsePlatformHelpers`

| Enabled by default | Supports autocorrection | Target Chef Version |
| --- | --- | --- |
| Enabled | Yes | All Versions |

## Examples


#### incorrect

```ruby
node['platform'] == 'ubuntu'
node['platform_family'] == 'debian'
node['platform'] != 'ubuntu'
node['platform_family'] != 'debian'
%w(rhel suse).include?(node['platform_family'])
node['platform'].eql?('ubuntu')
```

#### correct

```ruby
platform?('ubuntu')
!platform?('ubuntu')
platform_family?('debian')
!platform_family?('debian')
platform_family?('rhel', 'suse')
```

## Configurable attributes

<table>
<tbody><tr>
<th>Name</th>
<th>Default value</th>
<th>Configurable values</th>
</tr>
<tr>
<td style="text-align:center">Version Added</td>
<td style="text-align:center">5.6.0</td>
<td style="text-align:center">String</td>
</tr>
<tr><td style="text-align:center">Include</td>
<td style="text-align:center"><ul>
</ul>
</td>
<td style="text-align:center">Array</td>
</tr></tbody></table>
