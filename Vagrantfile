Vagrant.configure("2") do |config|
   config.vm.box = "ubuntu/bionic64"
   config.vm.network "forwarded_port", guest: 4200, host: 4200
   config.vm.network "public_network"
   config.ssh.insert_key = false

	$instaladores = <<-SCRIPT

        echo "------------------------------------------------------------------------------------------------------------------"
        echo ""
        echo "UPDATE"
        echo ""
        echo "------------------------------------------------------------------------------------------------------------------"
        sudo apt-get update
        if [ -z $(which curl) ]; then
         echo "------------------------------------------------------------------------------------------------------------------"
         echo ""
         echo "Instalando curl"
         echo ""
         echo "-----------------------------------------------------------------------------------------------------------------"
         sudo apt-get install -y curl
        fi

        if [ -z $(which node) ]; then
         echo "------------------------------------------------------------------------------------------------------------------"
         echo ""
         echo "Instalando NodeJS"
         echo ""
         echo "------------------------------------------------------------------------------------------------------------------"
         sudo curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
         sudo apt-get install -y nodejs
         sudo npm install -g node-gyp
        fi

        if [ -z $(which npm) ]; then
         echo "------------------------------------------------------------------------------------------------------------------"
         echo ""
         echo "Instalando NodeJS"
         echo ""
         echo "------------------------------------------------------------------------------------------------------------------"
         sudo apt-get install -y npm
        fi


        if [ -z $(which ng) ]; then
         echo "------------------------------------------------------------------------------------------------------------------"
         echo ""
         echo "instalando Angular"
         echo ""
         echo "------------------------------------------------------------------------------------------------------------------"
         sudo npm install mkdirp
         sudo npm install -g mkdirp
         mkdirp -v
         sudo npm install -g @angular/cli > /dev/null
        fi

        if !([ -d /home/vagrant/proyectos/ ]); then
          echo "------------------------------------------------------------------------------------------------------------------"
                 echo ""
                 echo "Creadndo Directorio y projecto"
                 echo ""
                 echo "------------------------------------------------------------------------------------------------------------------"
                  mkdir proyectos
        fi
                if !([ -d /lucho/ ]); then
                 sudo mkdir /lucho
                 sudo chmod -R 777 /lucho/
                fi

    SCRIPT

    $crear = <<-SCRIPT

              #! /bin/bash

             . /lucho/proyectos.sh
                echo "${proyectos[2]}"
                cd /home/vagrant/proyectos/
                   for i in "${proyectos[@]}"; do
                        bool="0"
                     for j in `ls`; do
                             if [ -d $j ]; then
                               if test "$i" = "$j"; then
                                    bool="1"
                                 fi
                             fi
                         done

                      if test "$bool" = "0"; then
                       ng new "$i"
                       echo "creado $i"
                       else
                       echo "$i vereficado"
                         fi
                    done


    SCRIPT

$borrar = <<-SCRIPT
              #! /bin/bash

             . /lucho/proyectos.sh

                cd /home/vagrant/proyectos/
               for i in `ls`; do
                    bool="0";
                       if [ -d $i ]; then
                                 for j in "${proyectos[@]}"; do
                                    if test "$i" = "$j"; then
                                      bool="1"
                                     fi
                                 done
                       fi

               done
SCRIPT

    $server = <<-SCRIPT

             . /lucho/proyectos.sh
                cd /home/vagrant/proyectos/
        bool="0";
        for i in `ls`; do
           if [ -d $i ]; then
               if test "$i" = "$SERVER_UP"; then
                bool="1"
               fi
           fi
         done
                if test "$bool" = "1"; then
                 cd /home/vagrant/proyectos/"$SERVER_UP"
                  sudo ng serve --host 0.0.0.0
                 else
                 echo "Servidor no encontrado"
                 fi

      SCRIPT
    config.vm.provision "shell", inline: $instaladores
    config.vm.provision "shell", inline: $borrar
    config.vm.provision "shell", inline: $crear
    config.vm.provision "shell", inline: $server, run: "always", privileged: true
    config.vm.synced_folder "../proyectos", "/home/vagrant/proyectos/"
    config.vm.synced_folder ".", "/lucho/"

end
