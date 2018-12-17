vagrant-audio-alpine-alsa
=========================

A minimal Vagrantfile that makes sound work on an Alpine Linux (guest).

Roughly based on [GeoffreyPlitt/vagrant-audio](https://github.com/GeoffreyPlitt/vagrant-audio).

Uses the ALSA sound driver system, and verified with Arch Linux (updated, 2018-12-17), VirtualBox 5.2.22.

- Do "vagrant up" to start everthing, then "vagrant ssh" to login to the VM.
- Do "alsabat-test.sh" and you should hear sound.
