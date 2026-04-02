# how backup jenkins
They server crash jobs,files,bulds everything gone hoe
go to linux sever check /var/lib/jekins tehn cp
incase local or windows avaible ./jenkins
# we can w=use cpo or sometime rsync tool
rsync tool install jenkins server then 
rsync works only linux server
# rsync -av /var/lib/jenkins/ /backup/jenkins/ a means copy everything files folder and v means verbose
# major advantage rsync what changes file change that only cp 
without rsync only cp all files cp again.

Rsync checks old backup vs new files — if same → skip — if changed → copy!





2. How do store/secure/handle secrets
    1. credential plugin
    2. environmental varibales
    3. hashicorp vault intergrated jenkins,ansible,terraform etc majority used
    4. thrid party: awd,azure secret manger etc hashicorp also third party

3. Latest versioin of jenkins
    Latest LTS Version: 2.541.3  mar 16 2025
    Latest LTS weekened: 2.557 

4. how to talk master to slave
go to jenkins-nodes-new-node-give slave pvt ip and ssh pvt key save 

5. most used daily used plugins
Git Plugin          ✅ Must have!
Pipeline Plugin     ✅ Must have!
Credentials Plugin  ✅ Must have!
Docker Plugin       ✅ Most projects
Slack Plugin        ✅ Most companies
slacvk means the pipeline fil or not othet something also.

in jnlp used to connect how?
Slave runs one java command with Master IP → Slave calls Master → Master accepts → Connected!
