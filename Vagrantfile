# Описание параметров ВМ
MACHINES = {
  # Имя DV "pam"
  :"pam" => {
              # VM box
              :box_name => "generic/centos8",
              #box_version
              :box_version => "4.3.12",
              #:box_version => "20210210.0",
              # Количество ядер CPU
              :cpus => 2,
              # Указываем количество ОЗУ (В Мегабайтах)
              :memory => 1024,
              # Указываем IP-адрес для ВМ
              :ip => "192.168.57.10",
            }
}

Vagrant.configure("2") do |config|
  MACHINES.each do |boxname, boxconfig|
    config.vm.provision :file, source: './files/', destination: '/home/vagrant/'
    # Отключаем сетевую папку
    config.vm.synced_folder ".", "/vagrant", disabled: true
    # Добавляем сетевой интерфейс
    config.vm.network "private_network", ip: boxconfig[:ip]
    # Применяем параметры, указанные выше
    config.vm.define boxname do |box|
      box.vm.box = boxconfig[:box_name]
      box.vm.box_version = boxconfig[:box_version]
      box.vm.host_name = boxname.to_s

      box.vm.provider "virtualbox" do |v|
        v.memory = boxconfig[:memory]
        v.cpus = boxconfig[:cpus]
      end
      box.vm.provision "shell", inline: <<-SHELL
          #Разрешаем подключение пользователей по SSH с использованием пароля
          sed -i 's/^PasswordAuthentication.*$/PasswordAuthentication yes/' /etc/ssh/sshd_config
          #Перезапуск службы SSHD
          systemctl restart sshd.service
          useradd otusadm && useradd otus
          echo "Otus2024!" | sudo passwd --stdin otusadm && echo "Otus2024!" | sudo passwd --stdin otus
          groupadd admin
          usermod otusadm -aG admin && usermod vagrant -aG admin && usermod root -aG admin
          cp ./files/scripts/login.sh /usr/local/bin/login.sh
          chmod +x /usr/local/bin/login.sh
          echo "auth       required     pam_exec.so /usr/local/bin/login.sh" >> /etc/pam.d/sshd
  	  SHELL
    end
  end
end
