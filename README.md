VM Arch 
=========

1. Install [Vagrant](https://developer.hashicorp.com/vagrant/downloads) on your system.
2. Install [Virtualbox](https://www.virtualbox.org/wiki/Downloads) on your system.
3. Personalize name, RAM, Disk and other options that are with the Tag `MODIFY`.
4. Staret up vagrant with Virtualbox:

    ```bash
        vagrant up
        # if change vagrant file run again
        vagrant up --provision
        # run for update the image box
        vagran box update
    ```