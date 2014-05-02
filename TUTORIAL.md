# Tutorial

## Deutsche Telekom AG

### Debian / Ubuntu

1. Install ruby

        apt-get install ruby1.9.1-full

2. Install chef

        gem1.9.1 install chef

3. May be you have to adjust the `$PATH` variable

        export PATH=$PATH:/var/lib/gems/1.9.1/bin/

4. Download the chef cookbook

        git clone ......./chef-os-hardening

5. Move hardening to `cookbooks`

        mkdir cookbooks
        mv chef-os-hardening cookbooks/os-hardening

6. Download some dependences for the os-hardening cookbook

        cd cookbooks
        git clone https://github.com/onehealth-cookbooks/sysctl
        git clone https://github.com/opscode-cookbooks/apt.git
        git clone https://github.com/gmiranda23/ntp.git
        git clone https://github.com/opscode-cookbooks/yum.git
        cd ..

7. Create `solo.rb`

    This file is used to specify the configuration details for chef-solo. So create a `solo.rb` that include the `cookbook_path`.

        cookbook_path "cookbooks"

8. Create `solo.json`

    Chef-solo does not interact with the Chef Server. Consequently, node-specific attributes must be located in a JSON file on the target system. Create the following `solo.json`.

        {
            "security" : {"suid_sgid": {
                "remove_from_unkown" : true,
                "system_whitelist" : []
                }
            },
            "run_list":[
                "recipe[os-hardening]"
            ]
        }


9. Run chef-solo

        chef-solo -c solo.rb -j solo.json




