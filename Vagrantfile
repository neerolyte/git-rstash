Vagrant::Config.run do |config|
  config.vm.box = "precise32"
  config.vm.box_url = "http://files.vagrantup.com/precise32.box"
  config.vm.share_folder "v-root", "/vagrant", "."

  config.vm.provision :shell, :inline => "apt-get install -y shunit2 git"
end
