Vagrant.configure("2") do |config|
  config.vm.box = "generic/alpine38"
  config.vm.provision :shell, :inline => $BOOTSTRAP_SCRIPT # see below

  # enable audio drivers on VM settings
  config.vm.provider :virtualbox do |vb|
    if RUBY_PLATFORM =~ /darwin/
      vb.customize ["modifyvm", :id, '--audio', 'coreaudio', '--audiocontroller', 'hda', '--audioout', 'on'] # choices: hda sb16 ac97
    elsif RUBY_PLATFORM =~ /mingw|mswin|bccwin|cygwin|emx/
      vb.customize ["modifyvm", :id, '--audio', 'dsound', '--audiocontroller', 'ac97', '--audioout', 'on']
    end
    vb.customize ["modifyvm", :id, '--audioout', 'on']
  end

end

$BOOTSTRAP_SCRIPT = <<EOF
  set -e # Stop on any error

  # Need to install the linux-vanilla kernel,
  # because the linux-virt kernel has no sound modules
  # FIXME: Maybe there's a simple enough way to get it to work with linux-virt...
  apk del linux-virt
  apk add linux-firmware-none linux-vanilla

  # Install ALSA and enable the service
  apk add alsa-utils alsa-lib alsaconf
  rc-update add alsa

  # Add users to ALSA group
  sed -i -e 's/^audio:x:18:$/audio:x:18:root,vagrant/' /etc/group

  # Unmute volume mixer on boot
  echo "#!/bin/sh
  amixer sset Master unmute
  amixer sset PCM unmute
  amixer set Master 100%
  amixer set PCM 100%" > /root/alsa-unmute-on-boot
  chmod +x /root/alsa-unmute-on-boot
  echo "@reboot /root/alsa-unmute-on-boot" > /etc/crontabs/root

  # Have to reboot for drivers to kick in, but only the first time of course
  if [ ! -f ~/alsa-reboot-runonce ]
  then
    touch ~/alsa-reboot-runonce
    sudo reboot
  fi
EOF
