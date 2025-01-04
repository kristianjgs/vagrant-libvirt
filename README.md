# Vagrant Libvirt Provider

![Vagrant Libvirt Logo](docs/_assets/images/logo.png?raw=true "Vagrant Libvirt")

[![Join the chat at https://gitter.im/vagrant-libvirt/vagrant-libvirt](https://badges.gitter.im/vagrant-libvirt/vagrant-libvirt.svg)](https://gitter.im/vagrant-libvirt/vagrant-libvirt?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
[![Build Status](https://github.com/vagrant-libvirt/vagrant-libvirt/actions/workflows/unit-tests.yml/badge.svg)](https://github.com/vagrant-libvirt/vagrant-libvirt/actions/workflows/unit-tests.yml)
[![Coverage Status](https://coveralls.io/repos/github/vagrant-libvirt/vagrant-libvirt/badge.svg?branch=main)](https://coveralls.io/github/vagrant-libvirt/vagrant-libvirt?branch=main)
[![Gem Version](https://badge.fury.io/rb/vagrant-libvirt.svg)](https://badge.fury.io/rb/vagrant-libvirt)

This is a [Vagrant](http://www.vagrantup.com) plugin that adds a
[Libvirt](http://libvirt.org) provider to Vagrant, allowing Vagrant to
control and provision machines via Libvirt toolkit.

**Note:** Actual version is still a development one. Feedback is welcome and
can help a lot :-)

Vagrant-libvirt Documentation is published at [https://vagrant-libvirt.github.io/vagrant-libvirt/](https://vagrant-libvirt.github.io/vagrant-libvirt/)

## QA status

We periodically test basic functionality for vagrant-libvirt on various distributions.
In the status badges below, build passing means that latest version of Vagrant + Vagrant-libvirt was installed correctly and `vagrant up` is working. Click the badge to review the action workflow.

[![](http://github-actions.40ants.com/vagrant-libvirt/vagrant-libvirt-qa/matrix.svg?only=Distribution%20Install.verify-install)](https://github.com/vagrant-libvirt/vagrant-libvirt-qa/actions/workflows/distro-install.yml)

## Index

<!-- vim-markdown-toc GFM -->

* [Installing](#installing)
  * [Latest development version](#latest-development-version)
* [Running](#running)
* [Development](#development)
* [Contributing](#contributing)

<!-- vim-markdown-toc -->

## Installing

Installation typically involves a number of distribution package dependencies to ensure that Libvirt is available.
Recommend that you follow the [installation guide](https://vagrant-libvirt.github.io/vagrant-libvirt/installation.html).

### Latest development version

If you want to try out the very latest development version you can download the gem package from the GitHub
rubygems package [repository](https://github.com/vagrant-libvirt/vagrant-libvirt/packages/1659776) under the
asserts. Unfortunately it's not yet possible to make the rubygem repositories in GitHub public.

To install provide the file directly to the install command:



To work on the `vagrant-libvirt` plugin, clone this repository out, and use
[Bundler](http://gembundler.com) to get the dependencies:

```shell
git clone https://github.com/kristianjgs/vagrant-libvirt.git
cd vagrant-libvirt
rake build

vagrant-libvirt 0 built to pkg/vagrant-libvirt-0.gem.
vagrant-libvirt 0 built to pkg/vagrant-libvirt-0.gem.


```

Installing using the output showed in this case is pkg/vagrant-libvirt-0.gem:

```shell
VAGRANT_DISABLE_STRICT_DEPENDENCY_ENFORCEMENT=1 vagrant plugin install pkg/vagrant-libvirt-0.gem
```

If those pass, you're ready to start developing the plugin.

Additionally if you wish to test against a specific version of vagrant you
can control the version using the following before running the tests:

```shell
export VAGRANT_VERSION=v2.2.14
bundle update && bundle exec rspec --fail-fast --color --format documentation
```

To run the acceptance tests which involve bringing up VMs and exercising
various functionality aspects run the following (warning, may have issues if
distro ruby is newer than supported by vagrant):

```shell
bundle exec rspec --fail-fast --color --format documentation --tag acceptance
```

**Note** rvm is used by the maintainers to help provide an environment to test
against multiple ruby versions that align with the ones used by vagrant for
their embedded ruby depending on the release. You can see what version is used
by looking at the current [unit tests](.github/workflows/unit-tests.yml)
workflow. By default if you have rvm installed and enabled it this project looks
to use ruby 3.1.2 and configures a separate gemset, both of which will be switched
to each time you enter the project directory. It should be considered sufficient
just to get any tests passing with your system ruby, and as long as you ensure
that the option to allow maintainers can update your PR, we will fix up any
issues with the remaining combinations.

You can test the plugin without installing it into your Vagrant environment by
just creating a `Vagrantfile` in the top level of this directory (it is
gitignored) that uses it. You can add the following line to your Vagrantfile
while in development to ensure vagrant checks that the plugin is installed:

```ruby
Vagrant.configure("2") do |config|
  config.vagrant.plugins = "vagrant-libvirt"
end
```

Or add the following to the top of the file to ensure that any required plugins
are installed globally:

```ruby
REQUIRED_PLUGINS = %w(vagrant-libvirt)
exit unless REQUIRED_PLUGINS.all? do |plugin|
  Vagrant.has_plugin?(plugin) || (
    puts "The #{plugin} plugin is required. Please install it with:"
    puts "$ vagrant plugin install #{plugin}"
    false
  )
end
```

Now you can use bundler to execute Vagrant:

```shell
bundle exec vagrant up --provider=libvirt
```

**IMPORTANT NOTE:** bundle is crucial. You need to use bundled Vagrant.

## Contributing

[![contributions welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat)](https://github.com/vagrant-libvirt/vagrant-libvirt/issues)

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

For future work take a look at [open issues](https://github.com/vagrant-libvirt/vagrant-libvirt/issues?state=open).

<!--
 # styling for TOC
 vim: expandtab shiftwidth=2
-->
