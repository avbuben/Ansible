# iptables-rules

##################################

На текущий момент данная роль реализует следующий набор iptables правил:

```bash
iptables -N chain_name  # create chain
iptables -I input_chain_name -p tcp -m tcp --dport port_number -m comment --comment "comment" -j chain_name # jump to dport
iptables -I chain_name -s source_address -j ACCEPT -m comment --comment "other comment" -j ACCEPT 
iptables -A chain_name -j DROP
```

К примеру нам необходимо реализовать следующий набор правил:
```bash
iptables -N custom_chain
iptables -I INPUT -p tcp -m tcp --dport 80 -m comment --comment "jump to custom_chain" -j custom_chain
iptables -I custom_chain -s 127.0.0.0/24 -j ACCEPT -m comment --comment localhost -j ACCEPT
iptables -I custom_chain -s 11.111.111.11/32 -j ACCEPT -m comment --comment test-server1 -j ACCEPT
iptables -I custom_chain -s 22.222.222.22/32 -j ACCEPT -m comment --comment test-server2 -j ACCEPT
iptables -A custom_chain -j DROP
```
Переменные будут выглядить следующим образом:

### Create custom iptables chains
```yml
custom_chain_list:
  - custom_chain
```
### Add rules for custom iptables chains
```yml
iptables_rules:
  - { chain: INPUT, port: "80", jump_chain: custom_chain, comment: jump to custom_chain, ip: "{{ custom_chain_ip_list }}" }
```
### Ip list for custom iptables chains
```yml
custom_chain_ip_list:
  - { ip: 127.0.0.0/24, comment: localhost }
  - { ip: 11.111.111.11/32, comment: test-server1 }
  - { ip: 22.222.222.22/32, comment: test-server2 }
```
#################################

пример вызова ansible playbook:
```yml
---

- hosts: all
  remote_user: test_user
  become: yes
  roles:
    - role: iptables-rules
```
