# -*- mode: ruby -*-
# vi: set ft=ruby :
# Vagrant configuration file for setting up a CentOS 9 VM with Nginx
Vagrant.configure("2") do |config|
  # Specify the base box to use
  config.vm.box = "generic/centos9s"
  # Forward port 80 on the guest to port 8888 on the host
  config.vm.network "forwarded_port", guest: 80, host: 8888, auto_correct: true
  # Sync local folder "./www-content" to "/var/www/html" on the guest using rsync
  config.vm.synced_folder "./www-content", "/var/www/html", type: "rsync", rsync__auto: true
  # Provision the VM with a shell script
  config.vm.provision "shell", inline: <<-SHELL
    # Install EPEL repository for extra packages
    yum install -y epel-release
    # Install Nginx, Midnight Commander, and rsync
    yum install -y nginx mc rsync
    # Disable SELinux temporarily
    setenforce 0
    # Disable SELinux permanently in config file
    sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config
    # Remove default Nginx configuration
    rm -f /etc/nginx/conf.d/default.conf
    # Create custom Nginx configuration for the site
    cat > /etc/nginx/conf.d/site.conf <<'EOF'
server {
    listen 80 default_server;
    server_name _;
    root /var/www/html;
    index index.html;
    location / {
        try_files $uri $uri/ =404;
    }
}
EOF

    # Start and enable Nginx service
    systemctl start nginx
    systemctl enable nginx

    # Disable and stop the firewall service
    systemctl disable firewalld
    systemctl stop firewalld
  SHELL
end

# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrant.configure("2") do |config|
  # Box configuration
  # config.vm.box = "generic/centos9s"

  # Forwarded port
# config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Synced folder
# config.vm.synced_folder "./www", "/var/www/html"

  # Disable default synced folder
  # config.vm.synced_folder ".", "/vagrant", disabled: true

  # Provider-specific configuration example for VirtualBox
  # config.vm.provider "virtualbox" do |vb|
  #   vb.gui = true
  #   vb.memory = "1024"
  # end

  # Provisioning with shell
  # config.vm.provision "shell", inline: <<-SHELL
  # yum install -y epel-release
  #   yum install -y nginx mc
  #   setenforce 0
  #  sed -ie 's/^SELINUX=.*$/SELINUX=disabled/' /etc/selinux/config
  #   cat /etc/selinux/config
  #   systemctl start nginx
  #   systemctl enable nginx
  #   systemctl disable firewalld
  #   systemctl stop firewalld
#   SHELL
# end