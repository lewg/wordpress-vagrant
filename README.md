Description
===========

A Virtualbox / Vagrant / Librarian / Chef setup for WordPress development. 


Requirements
------------
The idea is to provide a lightweight, consistent development environment. We do this by providing just enough instruction that the combination of VirtualBox, Vagrant, Librarian and Chef can handle building and configuring our dev box. So, you'll need those things! Here's the links:

* [VirtualBox](https://www.virtualbox.org/)
* [Vagrant](http://vagrantup.com) (handles the Chef requirement as well)
* [Ruby](http://www.ruby-lang.org/en/)
* [Librarian](https://github.com/applicationsonline/librarian)

**Note:** Although Vagrant is still available as a ruby gem, there are some dependency issues with Librarian. Also, it seems that the standalone package is going to be the officially supported version, so that's what I'm going with these days.

Instructions
------------

After cloning the reposity, have librarian-chef install the cookbooks

	librarian-chef install

Bring up your new box.

	vagrant up

Visit your site via IP to complete your install: [http://192.168.33.20](http://192.168.33.20)


Advice
------

What will really make this work for you is tinkering in the `Vagrantfile` definition to meet your development needs. I've made a few changes and additions to the defaults for our specific case of WordPress development.

### Base Box

To get you up and running quickly, I've specified a base box based on the 32-bit Ubuntu 10.04 version provided by the Vagrant site. If you're comfortable with Vagrant, by all means, provide your own! Here's the relevant lines:

	# Every Vagrant virtual environment requires a box to build off of.
	config.vm.box = "lucid32"

	# The url from where the 'config.vm.box' box will be fetched if it
	# doesn't already exist on the user's system.
	config.vm.box_url = "http://files.vagrantup.com/lucid32.box"

### Access

I've added a host-only IP of 192.168.33.20. If you plan to run multiple boxes at once, you may want to start changing them. Here's the relevant line:

	config.vm.network :hostonly, "192.168.33.20"

### Shared Folders

I've put commented lines in there for mapping folders into both the plugins and themes folders. Uncomment, adjust to your local paths, `vagrant reload` and your local folders are now being run by the WordPress install in your VM. Look for these lines:

    # Example Theme Mount
  	# config.vm.share_folder "my-theme", "/var/www/wordress/wp-content/themes/my-theme"

  	# Example Plugin
  	# config.vm.share_folder "my-plugin", "/var/www/wordpress/wp-content/plugins/my-plugin"


### WordPress.org Plugin and Themes

You can specify a list of plugins to automatically when the VM comes up. Debug Bar is in there as a default (you are using this for development, right?) but you can add and remove to match your setup. By default, just given a name, the defintion will attempt to guess the path to the plugin (on plugins.trac.wordpress.org) by replacing all the spaces with dashes, and removing some punctutation. Additionally, it will pull the latest version of the plugin based off the information provided by the plugin's readme.txt file. However, both the path and tag can be overridden ('trunk' being a special case, and choosing the trunk of the repository over a tag) The relevant lines in `Vagrantfile`: 

    # Include the list of plugins you'd like installed
    "org_plugins" => {
      'Debug Bar' => {},
      #'Bit.ly Service' => {
      #  'path' => 'bitly-service',
      #  'tag' => 'trunk'
      #}
    },

Like the plugin defintion and recipe above, there is also the ability to install themes from the wordpress.org theme repository. The syntax is the same as above, and by default the `Vagrantfile` has the two examples commented out:

    # Include the list of themes you'd like installed
    'org_themes' => {
      #'Toolbox' => {},
      #'Pagelines' => {
      #  'path' => 'pagelines',
      #  'tag' => '1.1.3'
      #}
    },

Unlike the mount points, you don't have to restart the VM if you add plugins or themes, just trigger a chef run with `vagrant provision`