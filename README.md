# rji-waylon [![Build Status](https://travis-ci.org/rji/puppet-waylon.svg?branch=master)](https://travis-ci.org/rji/puppet-waylon)
A Puppet module to deploy [Waylon][1] instances.

> This module is maintained by Puppet for internal purposes, and we have no plans for future feature development. It does not qualify for Puppet Support plans.
>
> [tier:maintenance-mode]

## Overview
This is a Puppet module to manage all aspects of deploying an instance of
Waylon, an application that displays the status of your Jenkins builds.

This module will install and manage Ruby (via rbenv), Waylon (via Rubygems),
Memcached, and Nginx, all on a single host.


## Compatibility
This module was developed to work with Debian 7 "Wheezy" running Puppet 3.x.
We plan to add support for additional operating systems in the future.


## Usage
To use the defaults as specified in `params.pp`, simply include the `waylon`
class in your Puppet manifests, like so:

```puppet
include waylon
```

You can also customize the deployment, to an extent:

```puppet
class { 'waylon':
  rbenv_install_path => '/usr/local/rbenv',
  ruby_version       => '2.1.5',
  unicorn_version    => '4.8.3',
  waylon_version     => '2.1.3',
}
```

Note that Waylon requires Ruby 2.1.x.


### Configuration
Ideally, all configuration should be in Hiera, as to keep the data separate
from the code. An example of this is shown below:

```yaml
waylon::config::refresh_interval: '120'
waylon::config::trouble_threshold: '0'
waylon::config::views:
  'My View':
    jenkins-dev:
      url: https://jenkins-dev.example.com
      username: 'waylon'
      password: 'topsecret'
      jobs:
        - 'Job 1'
        - 'Job 2'
```

For a complete reference of the YAML structure expected by Waylon, please see
the example config file located at `config/waylon.yml.example`.


## Development
This module was developed using Ruby 2.1.x and Puppet 3.x. After running
`bundle install`, there are a series of Rake tasks that can be executed to
validate syntax, perform linting, and execute tests:

```
$ bundle exec rake -T
rake beaker            # Run beaker acceptance tests
rake beaker_nodes      # List available beaker nodesets
rake build             # Build puppet module package
rake clean             # Clean a built module package
rake coverage          # Generate code coverage information
rake help              # Display the list of available rake tasks
rake lint              # Check puppet manifests with puppet-lint / Run pupp...
rake spec              # Run spec tests in a clean fixtures directory
rake spec_clean        # Clean up the fixtures directory
rake spec_prep         # Create the fixtures directory
rake spec_standalone   # Run spec tests on an existing fixtures directory
rake syntax            # Syntax check Puppet manifests and templates
rake syntax:hiera      # Syntax check Hiera config files
rake syntax:manifests  # Syntax check Puppet manifests
rake syntax:templates  # Syntax check Puppet templates
rake validate          # Check syntax of Ruby files and call :syntax / Vali...
```


### Testing with Travis CI
We use Travis CI to run syntax validation, linting, and spec tests, in that
order.


### Testing with Vagrant
For more extensive testing, we've included a [Vagrantfile](Vagrantfile) in the
root of the repository. You can bring in the dependencies and provision the
Vagrant box like so:

```
$ bundle install --path .bundle/cache
$ bundle exec rake spec_prep
$ vagant up
```

The app should now be available at <http://localhost:8080>, with a few Waylon
views already loaded (as defined in `spec/fixtures/hiera/common.yaml`).


### Contributing
For more information on contributing to rji-waylon, please see the
[CONTRIBUTING][2] doc in the root of this repo.


[1]: https://github.com/rji/waylon
[2]: CONTRIBUTING.md
