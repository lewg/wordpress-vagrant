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

You can specify a list of plugins and/or themes to automatically when the VM comes up. Debug Bar is in there as a default (you are using this for development, right?) but you can add and remove to match your setup. Plugins can be specified by their relative path on WordPress.org (http://http://wordpress.org/extend/plugins/**[PATH]**/). For readability you can also put in the name if it happens to match the URL after it's lowercased, and had spaces swapped for dashes (e.g. 'Debug Bar' becomes 'debug-bar'). A lot of WordPress plugins and themes adhere to that rule. The relevant lines in `Vagrantfile`: 

	# Include the list of plugins you'd like installed
    "org_plugins" => [ 'Debug Bar' ],
    # Include the list of themes you'd like installed
    'org_themes' => [],

Unlike the mount points, you don't have to restart the VM if you add plugins or themes, just trigger a chef run with `vagrant provision`