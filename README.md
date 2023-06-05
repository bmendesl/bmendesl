INSTALAÇÃO JFROG E MIGRAÇÃO

Atenção caso estiver realizando um upgrade favor realizar um backup da instancia e do banco de dados antes de qualquer procedimento.
    Realize o check de SSL para o Jfrog

1 - sudo yum update -y
2 - wget https://releases.jfrog.io/artifactory/artifactory-pro-rpms/artifactory-pro-rpms.repo -O jfrog-artifactory-pro-rpms.repo;
3 - sudo mv jfrog-artifactory-pro-rpms.repo /etc/yum.repos.d/;
4 - sudo yum update -y 
5 - yum install jfrog-artifactory-pro-7.55.10 -y
6 - cp /opt/jfrog/artifactory/app/artifactory/tomcat/lib/postgresql-*.jar /opt/jfrog/artifactory/var/bootstrap/artifactory/tomcat/lib/
7 - chown artifactory:artifactory /opt/jfrog/artifactory/var/bootstrap/artifactory/tomcat/lib/postgresql-*.jar
8 - vim /opt/jfrog/artifactory/var/etc/system.yaml
    - descomentar e/ou editar as variaveis na parte seção 'database':
        * type
        * driver
        * url (com o Endpoint ou DNS do banco)
        * username
        * password

Atenção, caso for uma instalação do zero e com um banco novo pule para o passo 11.

9 - caso o banco de dados já existir e já tiver dados copie a master key para a nova instancia
    9.1 - na instancia antiga
        9.1.1 - vim /opt/jfrog/artifactory/var/etc/security/master.key
    9.2 - na instancia nova
        9.2.1 - echo '<masterKey>' > /opt/jfrog/artifactory/var/etc/security/master.key"
10 - caso existir uma instancia antiga realizar o stop da mesma antes de subir o serviço da nova
    10.1 - na instancia antiga
        10.1.1 - systemctl stop artifactory.service
11 - start do serviço do artifactory
    11.1 - systemctl start artifactory.service
    11.2 - systemctl status artifactory.service
12 - validar se teve conecção com o banco
    12.1 - cd /opt/jfrog/artifactory/var/log
    12.2 - cat console.log | grep 'Connecting to'
13 - validação se a porta subiu
    13.1 - netstat -ntpl | grep 8082
