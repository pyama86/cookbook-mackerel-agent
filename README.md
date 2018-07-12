cookbook-mackerel-agent [![Build Status](https://travis-ci.org/mackerelio/cookbook-mackerel-agent.svg?branch=master)](https://travis-ci.org/mackerelio/cookbook-mackerel-agent)
=======================

This cookbook provides recipes to install and configure mackerel-agent.
mackerel-agent is a server monitoring agent for https://mackerel.io .

Requirements
============

- Chef 12.5 or higher
    - For AmazonLinux 2, Chef 14.3.36 or higher is required.
- Ruby 2.0

### Workarounds for old chef (11.x ~ 12.4.x)

On old chef (11.x ~ 12.4.x), chef may throw error below:

```
================================================================================
Recipe Compile Error in /var/chef/cache/cache/cookbooks/yum/resources/globalconfig.rb
================================================================================

NoMethodError
-------------
undefined method `property' for #<Class:0x00000003c9a088>
```

This is because `property` method in latest apt / yum cookbooks does not exist before chef 12.5.


#### Chef 12.0.x ~ 12.4.x

Use [compat_resource](https://github.com/chef-cookbooks/compat_resource) backports.

```ruby
# Berksfile
cookbook 'compat_resource'
```

#### Chef 11.x

Specify apt cookbook and yum cookbook to use version __prior to 4.0__ in `Berksfile`.

```ruby
# Berksfile
cookbook 'apt', '< 4.0'
cookbook 'yum', '< 4.0'
```

SYNPOSIS
========

```ruby
node.default['mackerel-agent']['conf']['apikey'] = 'Your API KEY' # required
node.default['mackerel-agent']['conf']['roles'] = ["My-Service:app", "Another-Service:db"] # optional

node.default['mackerel-agent']['conf']['plugin.metrics.vmstat'] = { # optional
  'command' => 'ruby /etc/sensu/plugins/system/vmstat-metrics.rb',
}

include_recipe 'mackerel-agent'
include_recipe 'mackerel-agent::plugins' # Option for installation of mackerel-agent-plugins package
```

Attributes
==========

The following attributes are set by default.
(CAUTION! node attribute namespace has changed since version 1.0.)

```ruby
default['mackerel-agent']['package-action'] = :upgrade
```

You can customize agent configuration via following attributes.
(These attributes are set to `nil` by default and agent uses their default configuration)

```ruby
default['mackerel-agent']['conf']['apikey']  = nil
default['mackerel-agent']['conf']['pidfile'] = nil # in Linux, agent's default: "/var/run/mackerel-agent.pid"
default['mackerel-agent']['conf']['root'] = nil # in Linux, agent's default: "/var/lib/mackerel-agent"
default['mackerel-agent']['conf']['verbose'] = nil # agent's default: false
default['mackerel-agent']['conf']['roles'] = nil
```

### Not to start mackerel-agent when you create a static image (like AMI)

```ruby
default['mackerel-agent']['start_on_setup'] = false
```

### Configure environment variable options
You can configure environment variable options via the following attributes.
(These all attributes are set to `nil` by default)

```ruby
default['mackerel-agent']['env_opts']['other_opts'] = nil
default['mackerel-agent']['env_opts']['auto_retirement'] = nil
default['mackerel-agent']['env_opts']['http_proxy'] = nil
default['mackerel-agent']['env_opts']['mackerel_agent_plugin_meta'] = nil
```

Development
===========

[Development Docuement](DEVELOPMENT.md)

LICENSE
=======

Copyright:: 2014 Hatena Co., Ltd.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
