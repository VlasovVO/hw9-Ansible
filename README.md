# hw9-Ansible

Для выполения ДЗ использовалась готовая роль (nginx)[https://galaxy.ansible.com/geerlingguy/nginx], одна из немногих работающих на galaxy =)

Перед этим установим роль `ansible-galaxy install -r requirements.yml`

Создадим свою роль, которая будет инициировать скаченную роль

```yml
---
- name: Install nginx
  hosts: all
  become: true

  roles:
    - nginx
```

Добавим запуск ansible в Vagrantfile для переопределения конфига по умолчанию:

```ruby
          box.vm.provision "ansible" do |ansible|
              ansible.playbook = "nginx.yml"
              ansible.groups = {
              "vb" => ["ansible"]  
              }
              ansible.extra_vars = {
              nginx_vhosts: [{
                listen: "8080",
                return: "301 http://localhost:80",
                server_name: "Homework"
                }]
              }
```
