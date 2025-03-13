Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"

  #I am using 8081 port, as my 8080 is ocupied by another service
  config.vm.network "forwarded_port", guest: 8080, host: 8080
  config.vm.network "forwarded_port", guest: 8443, host: 8443

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = 2
    config.vbguest.auto_update = false
  end

  config.vm.provision "shell", inline: <<-SHELL
    sudo apt update -y && sudo apt upgrade -y

    echo "Installing Java 17..."
    sudo apt install -y openjdk-17-jdk

    echo "Downloading Keycloak..."
    cd /home/vagrant
    curl -LO https://github.com/keycloak/keycloak/releases/download/26.1.3/keycloak-26.1.3.tar.gz
    tar -xvzf keycloak-26.1.3.tar.gz
    mv keycloak-26.1.3 keycloak
    rm keycloak-26.1.3.tar.gz

    echo "Setting permissions..."
    sudo chown -R vagrant:vagrant /home/vagrant/keycloak
    sudo chmod -R 755 /home/vagrant/keycloak

    echo "Creating Keycloak service..."
    sudo tee /etc/systemd/system/keycloak.service > /dev/null <<EOF
    [Unit]
    Description=Keycloak Server
    After=network.target

    [Service]
    User=vagrant
    WorkingDirectory=/home/vagrant/keycloak
    Environment="KEYCLOAK_ADMIN=admin"
    Environment="KEYCLOAK_ADMIN_PASSWORD=admin"
    ExecStart=/bin/bash -c '/home/vagrant/keycloak/bin/kc.sh start-dev --http-port=8080 --hostname=localhost --hostname-strict=false'
    Restart=always
    StandardOutput=journal
    StandardError=journal

    [Install]
    WantedBy=multi-user.target
    EOF

    echo "Reloading systemd and starting Keycloak..."

    echo "Adding Keycloak admin user..."
    export KEYCLOAK_ADMIN_PASSWORD=admin
    /home/vagrant/keycloak/bin/kc.sh bootstrap-admin user --username admin --password:env KEYCLOAK_ADMIN_PASSWORD

    echo "Reloading systemd and starting Keycloak..."
    sudo systemctl daemon-reload
    sudo systemctl enable keycloak
    sudo systemctl start keycloak

    echo "Waiting for Keycloak to start..."
    sleep 10
    echo "Keycloak is ready! Access it at http://localhost:8080 with admin/admin"
  SHELL

  config.trigger.after :up do |trigger|
    trigger.info = "🚀 Keycloak is ready! Access it at http://localhost:8080 with admin/admin"
  end
end
