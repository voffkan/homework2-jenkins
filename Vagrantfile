ENV["LC_ALL"] = "en_US.UTF-8"

$script = <<-EOF
export LC_ALL=en_US.UTF-8
sudo yum -y update
sudo yum -y install wget
sudo cp /vagrant/nginx.repo /etc/yum.repos.d/nginx.repo
sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo
sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
sudo yum -y install jenkins nginx net-tools policycoreutils-python java nano mc
sudo cp /vagrant/jenkins /etc/sysconfig/jenkins
sudo cp /vagrant/default.conf /etc/nginx/conf.d/default.conf
sudo semanage port -a -t http_port_t -p tcp 2222
sudo setsebool -P httpd_can_network_connect 1
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl enable nginx
sudo systemctl start nginx
EOF

Vagrant.configure("2") do |config|
  config.vm.define "test-vagrant" do |test|
    test.vm.box = "centos/7"
    test.vm.hostname = "voffka.box"
    test.vm.network :private_network, ip: "192.168.255.42"
    test.vm.network :public_network, bridge: "en0: Wi-Fi (AirPort)", use_dhcp_assigned_default_route: true

    test.vm.provision "shell", inline: $script
  end
end

