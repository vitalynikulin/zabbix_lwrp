# LWRP

This cookbooks provides next resources:
* [zabbix_lwrp_action](#zabbix_lwrp_action)
* [zabbix_lwrp_application](#zabbix_lwrp_application)
* [zabbix_lwrp_graph](#zabbix_lwrp_graph)
* [zabbix_lwrp_host](#zabbix_lwrp_host)
* [zabbix_lwrp_media_type](#zabbix_lwrp_media_type)
* [zabbix_lwrp_screen](#zabbix_lwrp_screen)
* [zabbix_lwrp_template](#zabbix_lwrp_template)
* [zabbix_lwrp_user_group](#zabbix_lwrp_user_group)

## zabbix_lwrp_action

zabbix_lwrp_action LWRP creates zabbix action with operations, conditions and messages.

### Actions
<table>
  <tr>
    <th>Action</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>sync</td>
    <td>Default action. Sync action description with one's in zabbix server</td>
  </tr>
</table>

### Attributes
<table>
  <tr>
    <th>Attribute</th>
    <th>Description</th>
    <th>Default</th>
  </tr>
  <tr>
    <td>name</td>
    <td><strong>Name attribute</strong>. Name of the action (required)</td>
    <td></td>
  </tr>
  <tr>
    <td>event_source</td>
    <td>Source of event for action, now only :trigger allowed</td>
    <td>:triggers</td>
  </tr>
  <tr>
    <td>escalation_time</td>
    <td>Delay between escalation steps</td>
    <td>60</td>
  </tr>
  <tr>
    <td>enabled</td>
    <td>Is action enabled?</td>
    <td>true</td>
  </tr>
  <tr>
    <td>message_subject</td>
    <td>Subject of action message</td>
    <td></td>
  </tr>
  <tr>
    <td>message_body</td>
    <td>Body of action message</td>
    <td>true</td>
  </tr>
  <tr>
    <td>send_recovery_message</td>
    <td>Send recovery message when conditions of action become false</td>
    <td>false</td>
  </tr>
  <tr>
    <td>recovery_message_subject</td>
    <td>Subject of recovery message</td>
    <td></td>
  </tr>
  <tr>
    <td>recovery_message_body</td>
    <td>Body of recovery message</td>
    <td></td>
  </tr>
  <tr>
    <td>operation</td>
    <td>Add an operation to action, see below. It's possible to add more then one operation</td>
    <td></td>
  </tr>
  <tr>
    <td>condition</td>
    <td>Add an condition to action, see below. It's possible to add more the one condition</td>
    <td></td>
  </tr>
</table>

Attributes for operation.
<table>
  <tr>
    <th>Attribute</th>
    <th>Description</th>
    <th>Default</th>
  </tr>
  <tr>
    <td>type</td>
    <td>Type of operation, can be :message or :command</td>
    <td>:message</td>
  </tr>
  <tr>
    <td>escalation_time</td>
    <td>Time to escalate to next step</td>
    <td></td>
  </tr>
  <tr>
    <td>start</td>
    <td>Start step, when this operation take place</td>
    <td></td>
  </tr>
  <tr>
    <td>stop</td>
    <td>Last step, when this operation take place</td>
    <td></td>
  </tr>
  <tr>
    <td>user_groups</td>
    <td>Zabbix user group names that'll receive message</td>
    <td></td>
  </tr>
  <tr>
    <td>message</td>
    <td>Message description, see below</td>
    <td>Use default actin message if empty</td>
  </tr>
</table>

Attributes for conditions.
<table>
  <tr>
    <th>Attribute</th>
    <th>Description</th>
    <th>Default</th>
  </tr>
  <tr>
    <td>type</td>
    <td>Type of condition, can be one of :trigger, :trigger_value, :trigger_serverity, :host_group, :maintenance</td>
    <td></td>
  </tr>
  <tr>
    <td>operator</td>
    <td>Operator for condition, can be one of :equal, :not_equal, :like, :not_like, :in, :gte, :lte, :not_in</td>
    <td></td>
  </tr>
  <tr>
    <td>value</td>
    <td>Value for condition.</td>
    <td></td>
  </tr>
</table>

It also possible to use short form of condition, like:
```ruby
  condition :trigger, :equal, '42th trigger name'
```

Attributes for message.
<table>
  <tr>
    <th>Attribute</th>
    <th>Description</th>
    <th>Default</th>
  </tr>
  <tr>
    <td>use_default_message</td>
    <td>Use default message from action</td>
    <td></td>
  </tr>
  <tr>
    <td>subject</td>
    <td>Message subject</td>
    <td></td>
  </tr>
  <tr>
    <td>message</td>
    <td>Message body</td>
    <td></td>
  </tr>
  <tr>
    <td>media_type</td>
    <td>Name of media type to use</td>
    <td>By default all media types are used</td>
  </tr>
</table>

### Examples
```ruby
zabbix_lwrp_action 'Some disturbance in force' do
  event_source :triggers
  operation do
    user_groups 'Ultimate Question Group'
  end

  condition :trigger, :equal, "#{node.fqdn}: Number of free inodes on log < 10%"
  condition :trigger_value, :equal, :problem
  condition :trigger_severity, :gte, :high
  condition :host_group, :equal, 'My Favorite Host Group'
  condition :maintenance, :not_in, :maintenance
end

```


## zabbix_lwrp_application

zabbix_lwrp_application lwrp creates application, items and triggers. You should think about items and triggers like nested
resources inside zabbix_lwrp_application lwrp.

### Actions
<table>
  <tr>
    <th>Action</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>sync</td>
    <td>Default action. Sync application description with one's in zabbix server</td>
  </tr>
</table>

### Attributes
<table>
  <tr>
    <th>Attribute</th>
    <th>Description</th>
    <th>Default</th>
  </tr>
  <tr>
    <td>name</td>
    <td><strong>Name attribute</strong>. Name of the application (required)</td>
    <td></td>
  </tr>
  <tr>
    <td>item</td>
    <td>Add an item to application, see below. It's possible to add more then one item</td>
    <td></td>
  </tr>
  <tr>
    <td>trigger</td>
    <td>Add an trigger to application, see below. It's possible to add more the one trigger</td>
    <td></td>
  </tr>
</table>

### Item attributes
Create/update zabbix item
<table>
  <tr>
    <th>Attribute</th>
    <th>Description</th>
    <th>Default</th>
  </tr>
  <tr>
    <td>key</td>
    <td><strong>Name attribute</strong>. Zabbix key for an item, should be unique.</td>
    <td></td>
  </tr>
  <tr>
    <td>type</td>
    <td>Type of zabbix check, possible values
      :zabbix, :trapper, :active, :calculated
    </td>
    <td></td>
  </tr>
  <tr>
    <td>name</td>
    <td>Descriptive name of zabbix check, shown in web-interface
    </td>
    <td></td>
  </tr>
  <tr>
    <td>frequency</td>
    <td>How often zabbix checks this item in seconds</td>
    <td>60</td>
  </tr>
  <tr>
    <td>history</td>
    <td>How many days store item's history</td>
    <td>7 days</td>
  </tr>
  <tr>
    <td>multiplier</td>
    <td>Set a multiplier for item</td>
    <td>0</td>
  </tr>
  <tr>
    <td>trends</td>
    <td>How many days store item's trends (archived history)</td>
    <td>365</td>
  </tr>
  <tr>
    <td>units</td>
    <td>Set a unit for item</td>
    <td></td>
  </tr>
  <tr>
    <td>value_type</td>
    <td>Type of gathered value, possible values
      :float, :character, :log_line, :unsigned_int, :text
    </td>
    <td>:unsigned_int</td>
  </tr>
</table>

### Trigger attributes
<table>
  <tr>
    <th>Attribute</th>
    <th>Description</th>
    <th>Default</th>
  </tr>
</table>

### Examples
```ruby
zabbix_lwrp_application "Test application" do
  action :sync

  item 'vfs.fs.size[/var/log,free]' do
    type :active
    name 'Free disk space on /var/log'
    frequency 600
    value_type :unsigned_int
  end

  trigger "Number #{node.fqdn} of free inodes on log < 10%" do
    expression "{#{node.fqdn}:vfs.fs.size[/var/log,free].last(0)}>0"
    severity :high
  end
end
```

## zabbix_lwrp_graph

### Examples
```ruby

zabbix_lwrp_graph 'Graph 1' do
  width 640
  height 480
  graph_items [:key => 'vfs.fs.size[/var/log,free]', :color => '111111']
end
```

## zabbix_lwrp_host

### Examples
```ruby
zabbix_lwrp_host node.fqdn do
  host_group 'My Favorite Host Group'
  use_ip true
  ip_address '127.0.0.1'
end
```

## zabbix_lwrp_media_type

### Examples
```ruby
zabbix_lwrp_media_type 'sms' do
  type :sms
  modem '/dev/modem'
end
```

## zabbix_lwrp_screen


### Examples
```ruby
zabbix_lwrp_screen 'Screen 1' do
  screen_item 'Graph 1' do
    resource_type :graph
  end
end
```

## zabbix_lwrp_template

### Actions
<table>
  <tr>
    <th>Action</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>add</td>
    <td>Default action. Add a template to node</td>
  </tr>
  <tr>
    <td>import</td>
    <td>Import templates from xml file to zabbix server</td>
  </tr>
</table>

### Attributes
<table>
  <tr>
    <th>Attribute</th>
    <th>Description</th>
    <th>Default</th>
  </tr>
  <tr>
    <td>path</td>
    <td><strong>Name attribute</strong>. Path to file for :import or name of template for :add action (required)</td>
    <td></td>
  </tr>
  <tr>
    <td>host_name</td>
    <td>Name of host new template to add</td>
    <td>FQDN of current node</td>
  </tr>
</table>

### Examples
```ruby
zabbix_lwrp_template '/tmp/zbx_templates_linux.xml' do
  action :import
end

zabbix_lwrp_template 'Linux_Template'
```

## zabbix_lwrp_user_group

### Actions
<table>
  <tr>
    <th>Action</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>create</td>
    <td>Default action. Create a new zabbix user group</td>
  </tr>
</table>

### Attributes
<table>
  <tr>
    <th>Attribute</th>
    <th>Description</th>
    <th>Default</th>
  </tr>
  <tr>
    <td>name</td>
    <td><strong>Name attribute</strong>. Name of new zabbix user group (required)</td>
    <td></td>
  </tr>
</table>

### Examples
```ruby
zabbix_lwrp_user_group 'Ultimate Question Group'
```