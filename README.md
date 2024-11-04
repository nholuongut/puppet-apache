# Puppet module: apache

![](https://i.imgur.com/waxVImv.png)
### [View all Roadmaps](https://github.com/nholuongut/all-roadmaps) &nbsp;&middot;&nbsp; [Best Practices](https://github.com/nholuongut/all-roadmaps/blob/main/public/best-practices/) &nbsp;&middot;&nbsp; [Questions](https://www.linkedin.com/in/nholuong/)
<br/>

This is a Puppet apache module from the second generation of nholuongut Puppet Modules.

Official git repository: http://github.com/nholuongut/puppet-apache

This module requires functions provided by the nholuongut Puppi module.

For detailed info about the logic and usage patterns of nholuongut modules read README.usage on nholuongut main modules set.

## USAGE - Module specific usage

* Install apache with a custom httpd.conf template and some virtual hosts

         class { 'apache':
           template => 'nholuongut/apache/httpd.conf.erb',
         }

         apache::vhost { 'mysite':
           docroot  => '/path/to/docroot',
           template => 'nholuongut/apache/vhost/mysite.com.erb',
         }


* Install mod ssl

        include apache::ssl


* Manage basic auth users (Here user joe is created with the $crypt_password on the defined htpasswd_file

        apache::htpasswd { 'joe':
          crypt_password => 'B5dPQYYjf.jjA',
          htpasswd_file  => '/etc/httpd/users.passwd',
        }


* Manage custom configuration files (created in conf.d, source or content can be defined)

        apache::dotconf { 'trac':
          content => template("site/trac/apache.conf.erb")
        }


* Add other listening ports (a relevant NameVirtualHost directive is automatically created)

        apache::listen { '8080': }


* Add other listening ports without creating a relevant NameVirtualHost directive

        apache::listen { '8080':
          $namevirtualhost = false,
        }


* Add an apache module and manage its configuraton

        apache::module { 'proxy':
          templatefile => 'site/apache/module/proxy.conf.erb',
        }


* Install mod passenger

        include apache::passenger


## USAGE - Basic management

* Install apache with default settings

        class { "apache": }

* Disable apache service.

        class { "apache":
          disable => true
        }

* Disable apache service at boot time, but don't stop if is running.

        class { "apache":
          disableboot => true
        }

* Remove apache package

        class { "apache":
          absent => true
        }

* Enable auditing without making changes on existing apache configuration files

        class { "apache":
          audit_only => true
        }

* Install apache with a specific version

        class { "apache":
          version =>  '2.2.22'
        }


## USAGE - Default server management

* Simple way to manage default apache configuration

        apache::vhost { 'default':
            docroot             => '/var/www/document_root',
            server_name         => false,
            priority            => '',
            template            => 'apache/virtualhost/vhost.conf.erb',
        }

* Using a source file to create the vhost

        apache::vhost { 'default':
	        source 		=> 'puppet:///files/web/default.conf'
	        template	=> '',
        }


## USAGE - Overrides and Customizations

* Use custom sources for main config file

        class { "apache":
          source => [ "puppet:///modules/lab42/apache/apache.conf-${hostname}" , "puppet:///modules/lab42/apache/apache.conf" ],
        }


* Use custom source directory for the whole configuration dir

        class { "apache":
          source_dir       => "puppet:///modules/lab42/apache/conf/",
          source_dir_purge => false, # Set to true to purge any existing file not present in $source_dir
        }

* Use custom template for main config file 

        class { "apache":
          template => "nholuongut/apache/apache.conf.erb",      
        }

* Define custom options that can be used in a custom template without the
  need to add parameters to the apache class

        class { "apache":
          template => "nholuongut/apache/apache.conf.erb",    
          options  => {
            'LogLevel' => 'INFO',
            'UsePAM'   => 'yes',
          },
        }

* Automaticallly include a custom subclass

        class { "apache:"
          my_class => 'apache::nholuongut',
        }


## USAGE - nholuongut extensions management 
* Activate puppi (recommended, but disabled by default)
  Note that this option requires the usage of nholuongut puppi module

        class { "apache": 
          puppi    => true,
        }

* Activate puppi and use a custom puppi_helper template (to be provided separately with
  a puppi::helper define ) to customize the output of puppi commands 

        class { "apache":
          puppi        => true,
          puppi_helper => "myhelper", 
        }

* Activate automatic monitoring (recommended, but disabled by default)
  This option requires the usage of nholuongut monitor and relevant monitor tools modules

        class { "apache":
          monitor      => true,
          monitor_tool => [ "nagios" , "monit" , "munin" ],
        }

* Activate automatic firewalling 
  This option requires the usage of nholuongut firewall and relevant firewall tools modules

        class { "apache":       
          firewall      => true,
          firewall_tool => "iptables",
          firewall_src  => "10.42.0.0/24",
          firewall_dst  => "$ipaddress_eth0",
        }

# 🚀 I'm are always open to your feedback.  Please contact as bellow information:
### [Contact ]
* [Name: Nho Luong]
* [Skype](luongutnho_skype)
* [Github](https://github.com/nholuongut/)
* [Linkedin](https://www.linkedin.com/in/nholuong/)
* [Email Address](luongutnho@hotmail.com)
* [PayPal.me](https://www.paypal.com/paypalme/nholuongut)

![](https://i.imgur.com/waxVImv.png)
![](Donate.png)
[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/nholuong)

# License
* Nho Luong (c). All Rights Reserved.🌟
