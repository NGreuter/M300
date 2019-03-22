Vagrant.configure(2) do |config|
  config.vm.define "VLCServer" do |vlc|
    #Auswahl der Box, der Host Name und Ports Einstellungen der VB
    vlc.vm.box = "ubuntu/xenial64"
    vlc.vm.hostname = "vlcserver"
    vlc.vm.network "forwarded_port", guest:8080, host:8080, host_ip: "0.0.0.0"
	vlc.vm.synced_folder "./media", "/home/media"
    #Virtualisierungs Programm, Name und Memory der VM auswählen
    vlc.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
    end
    #Mit diesem Schritt bringen wir die VM auf den neusten Stand
    vlc.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
	  sudo apt-get -y install vlc
	  sudo apt-get -y install vlc-plugin-fluidsynth vlc-plugin-pulse vlc-plugin-jack vlc-plugin-sdl
	  sudo apt-get -y install browser-plugin-vlc
	  sudo apt-get -y install libxvidcore4 libfaac0
	  vlc -vvv /home/media/media.mp4 --sout '#standard{access=http,mux=asf,dst=0.0.0.0:8080}'
	  #vlc -I http --http-host 192.168.50.4:80
      SHELL
    end
end
