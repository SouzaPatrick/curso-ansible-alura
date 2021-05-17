# curso-ansible-alura
Primeiro contato com ansible

Nesse tutorial estou usando uma maquina windows

Modulo 1:
- Primeiro passo sera instalar o Vagrant 
- Depois subir uma VM com Ansible, uma vez que, no windows nao temos como instalar o Ansible
- Gerar uma chave ssh para realizar as comunicacoes do ansible com as demais maquinas
Comando: ssh-keygen.exe -t rsa
    C:\Users\{Usuario}\{pasta_atual}\{nome_chave} 

### Acessar as maquinas
vagrant ssh {nome_da_maquina}
ou
ssh -i .\{chave} vagrant@{ip_da_maquina}

### Testar a comunicao entre as maquinas com Hello, World
ansible wordpress -u vagrant --private-key id_ansible -i ./configs/hosts -m shell -a 'echo Hello, World'

