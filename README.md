# Vagrant Boxes

If you don't find what you're looking for here, please see [veewee](https://github.com/jedi4ever/veewee) for more information.

## Getting Started

To build a new Vagrant VM image for use with Virtual Box you'll need a few
things:

- [x] Ruby (included with Mavericks)
- [ ] [Bundler](http://bundler.io/) for Ruby
- [ ] [Virtual Box](https://www.virtualbox.org/)
- [ ] [Vagrant](http://www.vagrantup.com/)
- [ ] A bit of time…

Core services in place clone this repository to a sensible location:

    $ git clone git@github.com:simpleweb/vagrant-boxes.git
    $ cd vagrant-boxes

    # Download the required gems
    $ bundle install --path=./.bundle

**Note:** A path has been set to avoid polluting your system gems, this will
keep all the required dependencies with in `./.bundle` directory within the
repo.


## Building a Vagrant VM image

It will help to find a template to work from for the distro you require – be
careful with blindly using the templates however or you may get unexpected
results.

    $ bundle exec veewee vbox template | grep -i ubuntu

Found your template? Define a new box and tell it to inherit from your chosen
template. Change `myubuntubox` to the name of your box, and the last string to
the name of the template (or leave it out for no template).

    $ bundle exec veewee vbox define 'myubuntubox' 'ubuntu-12.10-server-amd64'

At this stage I would suggest going through your box and customising the tasks
that happen. For more information check out [customising definitions](https://github.com/jedi4ever/veewee/blob/master/doc/customize.md).

When you're ready to build simply run the `build` method and watch the magic happen.

    $ bundle exec veewee vbox build 'myubuntubox'

## Export the VM image to a .box file

In order to use the box in Vagrant, we need to export the VM as a [base box](http://docs.vagrantup.com/v2/boxes.html) (e.g. export to the .box filetype):


    $ bundle exec veewee vbox export 'myubuntubox'


This is actually calling `vagrant package --base 'myubuntubox' --output 'boxes/myubuntubox.box'`.

The machine gets shut down, exported and will be packed in a `myubuntubox.box` file inside the current directory.

## Add the exported .box to Vagrant

To import it into Vagrant's box repository simply type:

    $ vagrant box add 'myubuntubox' 'myubuntubox.box'

The parameter 'myubuntubox' sets the name that Vagrant will use to reference the box (i.e. in the `Vagrantfile`).

## Use the added box in Vagrant

To use your newly generated box in a fresh project execute these commands:

    $ vagrant init 'myubuntubox'

If you already have a project running with Vagrant, open the `Vagrantfile` and change the value of `config.vm.box` to the new box name:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "myubuntubox"
end
```

See the [Vagrantfile machine settings](http://docs.vagrantup.com/v2/vagrantfile/machine_settings.html) for more details on setting up your `Vagrantfile` configuration.

Now start the new environment with `vagrant up` and log in with `vagrant ssh` to enjoy your new environment.
