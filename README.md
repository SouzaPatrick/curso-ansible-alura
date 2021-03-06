# curso-ansible-alura
Primeiro contato com ansible

Nesse tutorial estou usando uma maquina windows

Modulo 1:
- Primeiro passo sera instalar o Vagrant 
- Depois subir uma VM com Ansible, uma vez que, no windows nao temos como instalar o Ansible
- Gerar uma chave ssh para realizar as comunicacoes do ansible com as demais maquinas
Gerar chave ssh
```
ssh-keygen.exe -t rsa
```
Adicionar a pasta do peojeto
```
C:\Users\{Usuario}\{pasta_atual}\{nome_chave} 
```

### Acessar as maquinas
```
vagrant ssh {nome_da_maquina}
```
ou
```
ssh -i .\id_ansible vagrant@172.17.177.40
```

### Testar a comunicao entre as maquinas com Hello, World
```
ansible wordpress -u vagrant --private-key id_ansible -i ./configs/hosts -m shell -a 'echo Hello, World'
```
ou
Caso queira um comando mais verboso, dizendo exatamente tudo detalhadamente que ta acontescendo
```
ansible -vvvv wordpress -u vagrant --private-key id_ansible -i ./configs/hosts -m shell -a 'echo Hello, World'
```

Modulo 2:
- Movi a chave privada da pasta raiz para dentro da pasta de configs do ansible e realizei a sincronizacao dessa pasta com a maquina do ansible
- Criar o meu primeiro playbook, ou seja, as instrucoes que o anseble tera qu seguir
- Executar o primeiro playbook
```
ansible-playbook -i ./configs/hosts ./configs/provisioning.yml
```


Modulo 3:
- Voltei a chave privada para a raiz e retirei a sincronizacao, uma vez que, se au apagar a chave ou matar as instancias a chave foi apagada junto
- Iniciei a maquina como o ubuntu do professor e as mesas configs
- Melhoramos o playbook para que possa ter um loop e instalar todas as dependencias deuma vez e escrever pouco
```
---
- hosts: all
  tasks: 
    - name: 'Instala pacotes de dependencia para o sitema operacional'
      apt:
        name: "{{ item }}"
        state: latest
      become: yes
      with_items:
        - php5
        - apache2
        - libapache2-mod-php5
        - php5-gd
        - libssh2-php
        - php5-mcrypt
        - mysql-server-5.6
        - python-mysqldb
        - php5-mysql
```
- Tive que instalar o python 3.6, uma vez que, a maquina ubuntu/trusty64 veio com o 3.4 e era muito antigo para usar o ansible
- Melhoramos ainda mais o playbook pois a outra forma com 'with_items' foi descontinuada
```
---
- hosts: all
  tasks: 
    - name: 'Instala pacotes de dependencia para o sitema operacional'
      apt:
        name: 
          - php5
          - apache2
          - libapache2-mod-php5
          - php5-gd
          - libssh2-php
          - php5-mcrypt
          - mysql-server-5.6
          - php5-mysql
          - python-mysqldb
        state: latest
      become: yes
```

Modulo 4:
- Criar um banco de dados no playbook e tambem um usuario para acessa-lo

Modulo 5:
- Realizar a o download e instalacao do wordpress

Modulo 6:
- Criar um servidor especifico para o banco de dados e conectar o wordpress nele

Modulo 7:
- Criar variaveis de ambiente para deixar o codigo mais limpo
- Usar o Jinja2 com o uso de templates
- Reintalar o wordpress agr o 5.0 para que o wordpress rode, ja que a ultima versao nao aceita mais o php 5.5.9 que eh o instalado no curso

Modulo 8:
- Separar os codigos de forma que possam ser reaporveitados, atraves de roles