# Instalação do Omada 5 no Debian 11

## Requisitos:

- Debian 11 x64 - Instalação mínima + Servidor SSH
- OpenJDK 17
- MongoDB 4.4

## Instalação:

Editar o repositório e adicionar **contrib non-free**

    nano /etc/apt/sources.list

Instalar os softwares necessários:

    apt install openjdk-17-jre openjdk-17-jdk-headless gnupg curl wget autoconf make gcc

Instalar o **MongoDB 4.4** </BR>
(O Debian 11 não tem repositório para o MongoDB 4, por isso foi utilizado o repositório do Debian 10)

    curl -fsSL https://pgp.mongodb.com/server-4.4.asc | gpg -o /usr/share/keyrings/mongodb-server-4.4.gpg --dearmor
    echo "deb [ signed-by=/usr/share/keyrings/mongodb-server-4.4.gpg ] http://repo.mongodb.org/apt/debian buster/mongodb-org/4.4 main" | tee /etc/apt/sources.list.d/mongodb-org-4.4.list
    apt update
    apt install -y mongodb-org
    systemctl start mongod
    systemctl enable mongod
    
Verificar o status do MongoDB

    systemctl status mongod
    
Compilar **JSVC**

    mkdir -p /opt/tplink-sources && cd /opt/tplink-sources
    wget -c https://dlcdn.apache.org/commons/daemon/source/commons-daemon-1.3.4-src.tar.gz -O - | tar -xz
    cd commons-daemon-1.3.4-src/src/native/unix/
    sh support/buildconf.sh
    ./configure --with-java=/usr/lib/jvm/java-17-openjdk-amd64
    make
    ln -s /opt/tplink-sources/commons-daemon-1.3.4-src/src/native/unix/jsvc /usr/bin/
    
Instalação do **Omada**

    cd /opt/tplink-sources
    wget https://static.tp-link.com/upload/software/2023/202312/20231201/Omada_SDN_Controller_v5.13.22_Linux_x64.deb
    dpkg --ignore-depends=jsvc -i ./Omada_SDN_Controller_v5.13.22_Linux_x64.deb
    
Executar Omada Controller

      tpeap status -- Exibe o status da controladora;
      tpeap start -- Inicia a Controladora Omada;
      tpeap stop -- Para o serviço da Controladora Omada.

## Acesso:

Acesse o link abaixo pelo navegador e altere o IP do servidor Debian e utilize a porta 8043:

<https://IP_DO_DEBIAN:8043>
