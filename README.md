# Site Internet du Club Robot

Ce nouveau site a pour but de remplacer l'[ancien site](https://etud.insa-toulouse.fr/~club_robot/), totalement dépassé et pas mis à jour.

Il a plusieurs objectifs :
* le rendre bien plus maintenable
* avoir une interface plus avenante
* entreposer le site sur github pour un contrôle centralisé des ressources numériques du club
* écrire des articles facilement avec un joli affichage en Markdown

Pour ce faire, le site utilise un [thème jekyll](https://jekyllrb.com/), [Notepad](https://github.com/hmfaysal/Notepad). L'hébergement d'un site sur [<name>.github.io](https://clubrobotinsat.github.io/) l'oblige à être statique.

Pour pouvoir visualiser en local le site, il faut installer `bundler` (un truc en Ruby) ; il faut d'abord avoir python-pip installé.

Ensuite, ces commandes devraient tout installer :

```bash
sudo apt install ruby curl
sudo gem install rdoc bundler jekyll

cd /tmp
curl -O http://mirror.veriportal.com/gnu/gsl/gsl-2.5.tar.gz
tar xvzf gsl-2.5.tar.gz
cd gsl-2.5
./configure
make
sudo make install
sudo gem install --conservative --no-ri --no-rdoc gsl

bundler update
bundle install minitest
```

Enfin, en étant à la racine du répertoire `clubrobotinsat.github.io`, il faut exécuter la commande suivante :

```bash
bundler exec jekyll serve --safe
```

Le site est alors visualisable sur `localhost:4000` si tout s'est bien passé.

## Fichiers utiles

* `_config.yml` : ensemble des méta-données du site
* `Gemfile` : il faut rajouter les modules gem utilisés là-dedans
* `club-robot-insat.md` : le fichier affiché dans l'onglet **About**
* `_posts/` : les documents dedans servent de blog : on peut faire des articles / annonces pour le club et elles sont facilement répertoriables avec des tags
* `_drafts` : il faut commencer à rédiger les articles là-dedans, il faut ensuite appeler une commande pour les ûblier directement

## Ecrire des articles

1. Dans `_drafts`, copier un template en le renommant
2. Modifier les paramètres dans le header du fichier
3. Rédiger l'article
4. Une fois que tout est prêt, il est temps de publier :
    * vérifier localement que tout est bon avec les drafts : `bundler exec jekyll serve --watch --drafts`
    * publier un draft : `bundle exec jekyll publish _drafts/my-new-draft.md`
    * publier un draft avec une date spécifique : `bundle exec jekyll publish _drafts/my-new-draft.md --date 2018-01-18`



## Écrire des phrases pour la wall of shame:
1. Copier un des fichiers existants pour avoir le format et le renommer avec
la date de si tu la connais pas met une date au hazar ( recommendé 0000-01-01 ) et aprés le nom de la personne que l'a dit.
Tu peux definir un ```altDate: "texte"``` pour expliquer le contexte, par exemple J-2 avant la coupe.

2. Aprés, il faut écrire l'autheur qui est tuoi, la personne qui écrit la phrase sur le site, 
et le génie que c'est la personne qui l'a dit. 
