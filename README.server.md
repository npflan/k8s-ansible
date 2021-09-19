# Initializing an NPF kubernetes gameserver host

Installs prerequsites to run ansible, raw - so no real detection of state, but should be fine.
```bash
ansible-playbook initialize-ansible.yml -i hosts -K -k
```

Installs dependencies for running kubernetes
```bash
ansible-playbook deps.yml -i hosts -K -k
```

Adds the ubuntu user, with authorized_keys
```bash
ansible-playbook initial-server.yml -i hosts -K -k
```


If the server is not in the hosts file, you need to get its hostname and add it to it, either in serverrack2, or serverrack3, since those groups carry the bgp_ip of the top of rack switch.
Skip this step if all servers are in the serverrack groups already.

The process is still manual to convert the outputted debug lines into hosts lines.

Use this command to convert hardcoded ips in the hosts file to hostnames from the server:
```bash
ansible-playbook get-hosts-server.yml -i hosts -k -K
```


Join it into the cluster:
```bash
ansible-playbook kubeadm-server-join.yml -i hosts -K --private-key /keybase/team/servernpf/kube-ny/sshkey/server_npf.key
```


Enjoy
