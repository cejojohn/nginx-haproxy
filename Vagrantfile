Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"
  config.landrush.enabled = true
  config.landrush_ip.auto_install = true
  backend_instances = 2
# NGINX Instance to Server /campel-street
  (1..backend_instances).each do |instance_number|
    config.vm.define "backend-instance-#{instance_number}.com" do |host|
      host.vm.hostname = "backend-instance-#{instance_number}.com"
    end
  end
# NGINX Instance to decide routing logic
  config.vm.define "paramatta-friends.com" do |x|
    x.vm.hostname = "paramatta-friends.com"
  end
# SLB HAProxy Instance
  config.vm.define "paramatta-loadbalancer.com" do |y|
    y.vm.hostname = "paramatta-loadbalancer.com"
  end
# Local session
    config.vm.define "my-local-session.com" do |a|
      a.vm.hostname = "my-local-session.com"
    end
# Ansible provisioning
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "site.yaml"
    ansible.groups = {
    "nginx" => ["backend-instance-[1:2].com","paramatta-friends.com"],
    "haproxy" => ["paramatta-loadbalancer.com"],
    "nginx-router" => ["paramatta-friends.com"]
  }
  end
end
