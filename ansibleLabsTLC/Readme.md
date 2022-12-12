# TP Ansible

## Objectif du TP

Ce TP est une introduction pratique à Ansible. Le but est d'observer le déploiement automatisé à l'aide d'Ansible. Ce TP est donné dans le cadre du cours  [TLC](https://hackmd.diverse-team.fr/s/SJqu5DjSD) au [département d'Informatique et d'Electronique](https://istic.univ-rennes1.fr/) de l'[Université de Rennes 1](https://www.univ-rennes1.fr/). 

## Enoncé

- **Tâche 0**. Créer deux machines virtuelle (https://vm.istic.univ-rennes1.fr/ séléctionner **ubuntu22** comme image de base), l'accès au port 22 se fera au travers du [VPN](https://istic.univ-rennes1.fr/intranet/services#section-4). 

Une fois que vous avez créer ces machines, vérifier que vous avez accès à ces machines en ssh avec les instructions reçues par email. 



- **Tâche 1**.

En s'inspirant du playbooks disponible [ici](https://github.com/barais/demoAnsible/tree/main/lamp_ubuntu1804_2hosts_withroles), déployer cette simple application ansible sur vos vms. 

Sur votre portable : 

``bash
apt-get install ansible
git clone https://github.com/barais/demoAnsible
cd demoAnsible/lamp_ubuntu1804_2hosts_withroles
# edit the hosts to put the IP of your VMs and the login and password
nano -w hosts
ansible-playbook site.yml -i hosts
```



Essayer d'accéder à http://ADRESSE_IP_DE_VOTRE_VM_WEB/index.php


- **Tâche 2**.

Créer un nouveau playbook pour installer micok8S sur votre VM. 

Tout d'abord, créer la structure de votre playbook. Prenez le temps de lire les recommandations ici. 
https://docs.ansible.com/ansible/latest/tips_tricks/sample_setup.html

En utilisant les modules apt, snap et shell, créer un playbook pour installer microk8S. 

Je rappelle que microk8s s'installe manuellement avec les commandes suivantes.

- Install Docker

[Install Docker on ubuntu](https://docs.docker.com/engine/install/ubuntu/)

- Install microk8s

[See docs](https://microk8s.io/docs/getting-started)

```bash
sudo snap install microk8s --classic --channel=1.25
sudo usermod -a -G microk8s $USER
sudo chown -f -R $USER ~/.kube
microk8s start
microk8s status --wait-ready
```

Enable services

```bash
microk8s enable dns storage
microk8s enable dashboard ingress
microk8s enable registry
```

Pushing to this insecure registry may fail in some versions of Docker unless the
 daemon is explicitly configured to trust this registry. To address this we need
 to edit /etc/docker/daemon.json and add:

```json
{
  "insecure-registries" : ["localhost:32000"]
}
```

The new configuration should be loaded with a Docker daemon restart:

```bash
sudo systemctl restart docker
```

- **Tâche 3**.

Pour les plus gerriers, révisez votre playbook ansible pour utiliser le module suivant
https://galaxy.ansible.com/gepaplexx/microk8s
