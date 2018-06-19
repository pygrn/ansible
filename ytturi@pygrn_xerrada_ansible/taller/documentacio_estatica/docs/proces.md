# Taller - Realització de la tasca

Per tal de facilitar el procés, he extrapolat els processos que farem dins la tasca.

Si ho necessiteu, he afegit "Tips" que mostren quines comandes executariem en local.
Adicionalment, he afegit en cada pas alguns mòduls d'Ansible que us poden ser útils.

## Desplegament

El desplegament amb ansible és senzill. Símplement utilitzarem la següent comanda:

```
# Primer cal estar on hem configurat l'entorn
workon ansible
cd ~/projectes/pygrn_ansible

# Fem correr la tasca per un inventari en concret
ansible-playbook tasks/deploy_docs.yml -i inventory.yml
```

## Passos a realitzar en la tasca

### Tractament de peticions HTTP/S

Posarem un Nginx amb la configuració facilitada en el repositori de la xerrada:

> pygrn_xerrada_ansible/taller/docs_nginx.conf


Per tal de desplegar-ho, hem de pensar les passes que seguiriem en local i adaptar-ho a Ansible.

??? Note "Passes que seguiriem en local"
    1. Instalar Nginx amb `apt-get install nginx` 
    2. (**opcional**) Eliminar la configuració per defecte
    3. Afegir la configuració nova a "_sites-available_"
    4. Realitzar un link simbolic a "_sites-enabled_"

    ??? Note "Codi amb bash"
            sudo apt-get install nginx
            sudo rm /etc/nginx/sites-enabled/default
            sudo cp /.../pygrn_xerrada_ansible/taller/docs_nginx.conf /etc/nginx/sites-available/docs
            sudo ln -s /etc/nginx/sites-available/docs /etc/nginx/sites-enabled/docs 
        
        !!! Warning
            Com que utilitzem fitxers protegits cal utilitzar **sudo**!

    ??? Tip "Mòduls que ens poden ajudar"
        - APT
        - File
        - Copy

!!! Info
    No cal que compilem, utilitzem el mateix que podria venir amb un debian amb els repositoris de APT.


### Clonar el repositori

Aquesta documentació es troba en el repositori de github de la xerrada: [github:ytturi/pygrn_xerrada_ansible](https://github.com/ytturi/pygrn_xerrada_ansible).

Allà hi tenim el directori "`taller/documentacio_estatica`" on trobarem:

- El fitxer de configuració de mkdocs ("mkdocs.yml"),
- Els requeriments de pyhton ("requirements.txt") 
- La documentació en format Markdown en el directori "docs".

??? Note "Passes que realitzariem en local"
    1. Anar al directori de projectes
    2. Mirar si existeix el repositori
        1. Si no hi és el clonem amb `git clone https://github.com/ytturi/pygrn_xerrada_ansible.git`
        2. Si hi és, l'actualitzem amb `git pull`
        
    ??? Tip "Mòduls que ens poden ajudar"
        - GIT

### Configurar Python

Volem utilitzar MkDocs per realitzar el render de la pàgina. 
Però com fem (o hauriem de fer) amb la resta de projectes que treballem,
és millor instalar els requeriments en un virtualenv.

Per aquest motiu, el que farem serà utilitzar un virtualenv on hi instalarem els requeriments del repositori.

??? Note "Passes que realitzariem en local"
    1. Mirar si existeix la carpeta del virtualenv i activar-lo
        1. Si no existeix, en creem un de nou
    2. Anar a la carpeta del projecte
    3. Instalar els requeriments del projecte (del fitxer requirements.txt)

    ??? Tip "Mòduls que ens poden ajudar"
        - PIP

### Build de la documentació

Una vegada tenim l'entorn preparat, hem de realitzar build de la documentació per servir-la.
Realitzarem el build executant la comanda `mkdocs build -d "../built_docs"` des del virtualenv anterior.

??? Tip "Executar una comanda des d'un virtualenv sense ser-hi"
    Per tal d'executar una comanda des d'un virtualenv en una sola línia podem cridar-la utilitzant
    tot el _path_ del virtualenv.

    Per exemple si tenim el virtualenv en el directori: `/home/usuari/.virtualenvs/documentacio`

    Podem utilitzar: `/home/usuari/.virtualenvs/documentacio/bin/mkdocs`
 

??? Question "Sempre cal fer build de la documentació?"

    No sempre cal fer build. Però en el repositori tenim Markdown, i els fitxers a servir són HTML.

    Com sabem si s'han modificat?

    Aquests casos són més difícils de tractar, pel que millor fem build cada vegada.

!!! Important
    Per tal de tocar més funcionalitats d'Ansible, com els condicionals i els "register", 
    afegirem la següent condició en aquest pas:

    - Si ja teniem el repositori clonat i no s'ha d'actualitzar

!!! Important
    Si no ho heu fet fins ara, sería interessant que utilitzessiu variables per els directoris afectats:

    - Virtualenv
    - Documentació
    - HTML

    Si ho feu, podreu executar la comanda utilitzant-les, com ara:

    `{{ venv }}/bin/mkdocs build -c {{ docs }}/mkdocs.yml  -d {{ projectes }}/built_docs`

??? Note "Passes que realitzariem en local"
    1. En el pas de "[Clonar el repositori](#clonar-el-repositori)" ja sabrem si s'ha actualitzat o
       ha canviat algun fitxer del repositori. Si no ha canviat res, no farem aquest pas.
    2. Realitzarem el build executant la comanda `mkdocs build -d "../built_docs"`

    ??? Tip "Mòduls que ens poden ajudar"
        - SCRIPT
        - SHELL
        - COMMAND

### Actualització dels fitxers a servir

Aquesta part és la més senzilla. Com que tot ho servim via Nginx,
símplement cal que els fitxers a servir es modifiquin.

Ho podem fer de varies maneres:

- Després de fer build, movem o copiem els fitxers en el directori que serveix el NginX.
- Podem assegurar-nos de que hi ha un link de la carpeta dels fitxers a la carpeta que serveix NginX.

??? Tip "Mòduls que us poden ajudar"
    - FILE
    - COMMAND


