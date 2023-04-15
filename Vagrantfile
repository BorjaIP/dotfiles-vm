# -*- mode: ruby -*-
# vi: set ft=ruby :

# MODIFY
bootstrap = <<SCRIPT
  useradd -m -s /bin/bash -U borja -u 666 --groups wheel || true
  echo "%borja ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/borja
SCRIPT

guest_utils = <<SCRIPT
  pacman -Rs --noconfirm virtualbox-guest-utils-nox || true
  pacman -Qq virtualbox-guest-utils > /dev/null 2>&1 || pacman --noconfirm --needed --noprogressbar -S virtualbox-guest-utils
SCRIPT

# MODIFY
setup_config = <<SCRIPT
  su -c "mkdir -p /home/borja/.config"
  su -c "cp -r .config /home/borja/"
  su -c "mv -f .zshenv /home/borja/"
  su -c "chown -R borja:borja /home/borja"
SCRIPT

Vagrant.configure("2") do |config|

  unless Vagrant.has_plugin?("vagrant-disksize")
    raise  Vagrant::Errors::VagrantError.new, "vagrant-disksize plugin is missing. Please install it using 'vagrant plugin install vagrant-disksize' and rerun 'vagrant up'"
  end

  # Official Arch linux distribution https://archlinux.org/download/
  config.vm.box = "archlinux/archlinux"
  config.vm.hostname = "Arch"
  config.disksize.size = "10GB" # MODIFY
  
  config.vm.provider "virtualbox" do |vb|
    vb.name = "borja" # MODIFY
    # Display the VirtualBox GUI when booting the machine
    # vb.gui = true

    vb.cpus = 4 # MODIFY
    vb.memory = "8192"  # MODIFY

    vb.customize ["modifyvm", :id, "--clipboard-mode", "bidirectional"]
    vb.customize ["modifyvm", :id, "--vram", "64"]
  end

  config.vm.provision "Creating user", type: "shell", inline: "#{bootstrap}"
  config.vm.synced_folder "C:/Users/Borja/Pictures", "/home/borja/pictures" # MODIFY

  # virtualbox-guest-utils
  config.vm.provision "VBox guest-utils", type: "shell", inline: "#{guest_utils}"

  # Installing packages
  config.vm.provision "Installing packages", type: "shell", privileged: false, path: "install.sh"

  # Provision system files
  config.vm.provision "Zsh", type: "file", source: "config", destination: ".config"
  config.vm.provision "Zshenv", type: "file", source: "zshenv", destination: ".zshenv"

  # Config
  config.vm.provision "Config", type: "shell", privileged: true, inline: "#{setup_config}"
  
end