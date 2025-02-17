+++
title = "PowerShellGuardInterpreter"

+++

<!-- This content is automatically generated. See https://github.com/chef/chef-web-docs/blob/main/generated/README.md -->

The department is: `PowerShellGuardInterpreter`

The full name of the cop is: `Chef/Modernize/PowerShellGuardInterpreter`

| Enabled by default | Supports autocorrection | Target Chef Version |
| --- | --- | --- |
| Enabled | Yes | 13.0+ |

## Examples


#### incorrect

```ruby
powershell_script 'Create Directory' do
  code "New-Item -ItemType Directory -Force -Path C:\mydir"
  guard_interpreter :powershell_script
end

batch 'Create Directory' do
  code "mkdir C:\mydir"
  guard_interpreter :powershell_script
end
```

#### correct

```ruby
powershell_script 'Create Directory' do
  code "New-Item -ItemType Directory -Force -Path C:\mydir"
end

batch 'Create Directory' do
  code "mkdir C:\mydir"
end
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
<td style="text-align:center">5.9.0</td>
<td style="text-align:center">String</td>
</tr>
<tr><td style="text-align:center">Include</td>
<td style="text-align:center"><ul>
</ul>
</td>
<td style="text-align:center">Array</td>
</tr></tbody></table>
