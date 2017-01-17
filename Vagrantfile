#-*- mode: ruby -*-
# vi: set ft=ruby :

$linux_script = <<SCRIPT
  echo debconf shared/accepted-oracle-license-v1-1 select true | sudo debconf-set-selections
  echo debconf shared/accepted-oracle-license-v1-1 seen true | sudo debconf-set-selections
  echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" | sudo tee /etc/apt/sources.list.d/webupd8team-java.list
  echo "deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" | sudo tee -a /etc/apt/sources.list.d/webupd8team-java.list
  sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys EEA14886
  sudo apt-get update
  sudo apt-get install --yes oracle-java8-installer

  sudo apt-get install --yes git
  sudo apt-get install --yes software-properties-common

  sudo add-apt-repository --yes ppa:andrei-pozolotin/maven3
  sudo apt-get update
  sudo apt-get install --yes maven3
  sudo apt-get install --yes libfontconfig1
SCRIPT

$win_script = <<-SCRIPT
  $ChocoInstallPath = "$env:SystemDrive\\ProgramData\\Chocolatey\\bin"

  if (!(Test-Path $ChocoInstallPath)) {
    iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))
  }

  New-Item HKLM:\\SOFTWARE\\Policies\\Microsoft\\Windows -Name WindowsUpdate
  New-Item HKLM:\\SOFTWARE\\Policies\\Microsoft\\Windows\\WindowsUpdate -Name AU
  New-ItemProperty HKLM:\\SOFTWARE\\Policies\\Microsoft\\Windows\\WindowsUpdate\\AU -Name NoAutoUpdate -Value 1

  $ChocoCommand = "$ChocoInstallPath\\choco install -y git jdk8 maven"
  iex "& $ChocoCommand"
SCRIPT

Vagrant.configure(2) do |config|
  config.vm.provider "virtualbox" do |v|
    v.memory = 8192
    v.cpus = 4
    v.gui = false
    v.customize ["modifyvm", :id, "--ioapic", "on"]
    v.customize ["modifyvm", :id, "--vram", "128"]
    v.customize ["modifyvm", :id, "--mouse", "usbtablet"]
    v.customize ["modifyvm", :id, "--accelerate3d", "on"]
    v.customize ["modifyvm", :id, "--usb", "on"]
    v.customize ["modifyvm", :id, "--usbehci", "off"]
  end
  
  config.vm.box = "ARTACK/debian-jessie"
  
  (1..25).each do |i|
    config.vm.define "linux#{i}" do |lin|
      lin.vm.provision "shell", inline: $linux_script
      lin.vm.hostname = "linux#{i}.vagrant"
      lin.vm.network "public_network"
    end

    config.vm.define "windows#{i}" do |win|
      config.vm.box = "opentable/win-2012r2-standard-amd64-nocm"
      win.vm.guest = :windows
      win.vm.communicator = "winrm"
      win.vm.provision "windows provision", type: "shell", privileged: true, inline: $win_script
      win.vm.network "public_network"
    end
  end
end
