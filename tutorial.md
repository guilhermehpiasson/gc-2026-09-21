# 1 - Execução de um container

docker run -it `
  --name ansible `
  -v /var/run/docker.sock:/var/run/docker.sock `
  -v "${PWD}:/ansible" `
  -w /ansible `
  ubuntu:24.04 `
  bash
  

# 2 - Atualização de Dependencias do container
  
apt update

# 3 - Instalação de Dependencias do container
apt install -y \
  ansible \
  docker.io \
  python3 \
  python3-pip
  
# 4 - Instalação de  Dependencias adicionais no container
 
 pip3 install docker --break-system-packages 
 ansible-galaxy collection install community.docker
 
# 5 - Atualização da diretiva de hosts
 
cat > /ansible/hosts <<'EOF'
[docker]
localhost ansible_connection=local
EOF
 
# 6 - Atualização do ansible playbook

---
- hosts: docker

  tasks:
    - name: Baixar e rodar o contêiner Nginx
      community.docker.docker_container:
        name: nginx_server
        image: nginx:latest
        state: started
        ports:
          - "8080:80"
		  
		  
# 7 - Execução do Ansible Playbook

ansible-playbook -i hosts provision_nginx.yml

