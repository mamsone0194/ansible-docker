## Création d'un docker de test

```
docker pull httpd
docker run -d --name apache -p 8888:80 httpd
```

## Configuration élémentaire d'Ansible
Fichier *ansible.cfg* :
```
[defaults]
inventory = inventaire
```

## Création de l'inventaire
```
mkdir inventaire
vi inventaire/hosts
-> [web]
-> apache
```

## Configuration de la connexion
-> Fichier group_vars/web.yml :
ansible_connection: docker
ansible_user: root

## Test
NB: l'image ***httpd*** ne contenant pas l'interpréteur Python, il n'est pas possible d'utiliser immédiatement un module ansible du type *ping* !
On peut néanmoins faire un test avec le module ***raw*** :
```
$ ansible -m raw -a "ls -l" web
apache | CHANGED | rc=0 >>
total 40
drwxr-xr-x 2 root root 4096 Apr 20 09:50 bin
drwxr-xr-x 2 root root 4096 Apr 20 09:50 build
drwxr-xr-x 2 root root 4096 Apr 20 09:50 cgi-bin
drwxr-xr-x 4 root root 4096 Apr 20 09:50 conf
drwxr-xr-x 3 root root 4096 Apr 20 09:50 error
drwxr-xr-x 2 root root 4096 Apr 20 09:50 htdocs
drwxr-xr-x 3 root root 4096 Apr 20 09:50 icons
drwxr-xr-x 2 root root 4096 Apr 20 09:50 include
drwxr-xr-x 1 root root 4096 Apr 29 13:10 logs
drwxr-xr-x 2 root root 4096 Apr 20 09:50 modules
```

## Installation de python3 sur le docker
```
ansible -m raw -a "apt update" web
ansible -m raw -a "apt install -y python3" web
ansible -m ping web
```
