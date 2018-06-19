# 06/2018 Python Girona - Xerrada + Taller Ansible

Xerrada de familiarització amb Ansible.

Issue: [https://github.com/pygrn/xerrades/issues/20](https://github.com/pygrn/xerrades/issues/20)

## Slides

Aquestes slides s'han fet utilitzant [Jupyter](http://jupyter.org).

Podeu visualitzar-les mitjançant [notebook](http://nbviewer.jupyter.org/github/ytturi/pygrn_xerrada_ansible/blob/master/slides/slideshow.ipynb)
 o [slides](http://nbviewer.jupyter.org/format/slides/github/ytturi/pygrn_xerrada_ansible/blob/master/slides/slideshow.ipynb#/)

## Taller i documentació

Realització d'una tasca d'Ansible i desplegament.

Disponibles en Github Pages: [https://ytturi.github.io/pygrn_xerrada_ansible/](https://ytturi.github.io/pygrn_xerrada_ansible/)

Si voleu podeu fer servir Docker com a suport durant el taller. Per aixecar-lo només heu de fer

```
$ docker-compose up
```
, i us aixecarà un container amb l'sshd preconfigurat (root:root).

Mapeig de ports
- 22 -> 2222
- 80 -> 8080
- 443 -> 8443
, de forma que si feu `ssh 0 -p2222 -lroot` estareu accedint al vostre container
