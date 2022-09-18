# Proyecto 
El proyecto **aws-ansible-deployment** es un ejemplo de ejecución de ansible desde una instancia Amazon Linux 2

## Crear repositorio
- aws-ansible-deployment

## Crear instancia
- Amazon Linux 2
- Kernel 5.10 AMI 2.0.20220805.0 arm64 HVM 
- gp3 / 8GB
- ami-05f3141013eebdc12
- t4g.nano
- username-labs.pem
- SSH Acces. Port:22.
- HTTP Acces. Port:80.
- Rol IAM: ansible-deployment

## Habilitar repositorio EPEL
```bash
amazon-linux-extras install epel
yum update -y
```

## Instalar Ansible + Nginx + Git
```bash
yum install ansible -y
yum install nginx -y
yum install git -y
```

## Instalar node
```bash
cd ~
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
source .bashrc
echo $NVM_DIR
source ~/.bashrc
. ~/.bash_profile
nvm --version
nvm install v16
node -e "console.log('Running Node.js ' + process.version)"
```

## Instalar Express
```bash
mkdir server && cd server
npm install express
```

## Ejecutar aplicación
- Subir archivo app.js a la instancia EC2 a la carpeta server
- Ejecutar la aplicación:
```bash
node app.js
```

## Configurar Nginx
Modificar el bloque por defecto para que incluya el contenido de [server.conf](server.conf)
```bash
sudo nano /etc/nginx/nginx.conf
```
Iniciar Nginx y habilitar incio
```bash
systemctl start nginx 
systemctl enable nginx
```
Verificar estado de Nginx
```bash
sudo systemctl status nginx.service
```

## Configurar Github
Generar llave en la instancia EC2
```bash
ssh-keygen -t rsa -b 4096 -C <your_email@example.com>
eval "$(ssh-agent -s)"
```
Agregar llave de despliegue en Github
```bash
cat .ssh/id_rsa.pub
```
Validar la clonación del repositorio
```bash
cd /tmp && git clone git@github.com:gfourmis/aws-ansible-deployment.git && rm -rf aws-ansible-deployment
```

Crear webhook en Github apuntando a http://my.ip.ec2.instance/

## Configurar Ansible
Crear grupo de instancias
```bash
sudo nano /etc/ansible/hosts
```

# Enlaces
1. [Automate Ansible playbook deployment with Amazon EC2 and GitHub](https://aws.amazon.com/es/blogs/infrastructure-and-automation/automate-ansible-playbook-deployment-amazon-ec2-github/)
2. [Installing a custom version of NVM and Node.js](https://help.dreamhost.com/hc/en-us/articles/360029083351-Installing-a-custom-version-of-NVM-and-Node-js)
3. [How To Write Ansible Playbooks](https://www.digitalocean.com/community/tutorial_series/how-to-write-ansible-playbooks)
4. [Intro to playbooks](https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html)
5. [Ansible Tutorial for Beginners: Playbook, Commands & Example](https://www.guru99.com/ansible-tutorial.html)