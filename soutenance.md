# Support soutenance : Projet 3 Ohmyfood

## <a id="i">Index</a>

[1 - Contexte](#1)

[2 - Mettre en place son environnement Front-End](#2)

[2.1 - Environnement Front-End](#2.1)

[2.1.1 - Sass](#2.1.1)

[2.1.2 - Git](#2.1.2)

[3 - Présentation du site sur Github Pages](#3)

[4 - Animations](#4)

[4.1 - Animation 1 : Boutons](#4.1)

[4.2 - Animation 2 : Coeur](#4.2)

[4.3 - Animation 3 : entrée progressive des cartes](#4.4)

[4.4 - Animation 4 : Bloc vert + checkmark](#4.4)

[4.5 - Animation 5 : Loader](#4.5)

[5 - Points d'amélioration](#5)

[6 - Q&A](#6)

[7 - Débrief](#7)

Pour gagner du temps, les points suivants seront partagés à l'écran :

- Partie **2.1.2 - Git**
- Partie **4 - Animations**
___

## <a id="1">1 - Contexte </a>

**Ohmyfood :** service web de réservation de plats dans les restaurants.
**Objectif :** intégrer les maquettes du prototype de Ohmyfood.

___
## <a id="2">2 - Mettre en place son environnement Front-End</a>

(Estimé 2 min - réel 2min40)

### <a id="2.1">2.1 - Environnement Front-End</a>

**Local** : VS Code, Sass, Git
**Remote** : Github / Github Pages

### <a id="2.1.1">2.1.1 - Sass</a>

Utilisés :

- Arborescence 5-1 (pas de /vendors ni /themes)
- Partials et index
- Mixins
- Peu d'imbrication

### <a id="2.1.2">2.1.2 - Git</a>

**Structure initiale prévue :**

Méthode suggérée par mon mentor (pour un projet solo).

>**MASTER** Version mobile statique [1 commit amendé]
>|
>|----- **Branche 1** : Feat_heart_animation [1 commit amendé]
>|
>|----- **Branche 2** : Feat_menu_animation [1 commit amendé]
>|
>|----- **Branche 3** : Feat_checkmark_animation [1 commit amendé]
>|
>|----- **Branche 4** : Feat_loader_animation [1 commit amendé]

Rebase des 4 branches sur *master* à la fin du projet.

**Structure réelle :**

> **MASTER** [24 commits]
>|
>|----- **Branche 1** : feat_desktop-version [31 commits]
>(1er commit de la branche = dernier commit de master)

[Retour à l'index](#i)
___

## <a id="3">3 - Présentation du site sur Github Pages</a>

(Estimé 3min - Réel 3 min)

- Mobile
- Tablette
- Desktop

[Retour à l'index](#i)
___

## <a id="4">4 - Animations</a>

(Estimé : 10 min - Réel 11 min)

### <a id="4.1">4.1 - Animation 1 : Boutons</a>

**Points d'intérêt :**
Pas de `background-image: linear-gradient()` mais une `box-shadow: inset` avec transition
Un pseudo élément à opacité 0 -> 1 avec 2nde `box-shadow` pour éviter le calcul de 2 transitions sur `box-shadow`.
Référence[^1]

**Extraits de code :**

```scss
.cta__btn {
    position: relative;
    margin-top: 2em;
    margin-bottom: 2em;
    padding: 0 1em 0 1em;
    height: 50px;
    border: none;
    border-radius: 2em;
    color: white;
    font-weight: a.$fw-bold;
    cursor: pointer;
    box-shadow: a.$box-shadow;
    background-color: a.$clr-primary;
    box-shadow: 0 50px 20px -35px a.$clr-secondary inset;
    transition: box-shadow .5s ease-in-out;
    
    @include a.flex-center();
}

.cta__btn::after {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    bottom:0;
    right: 0;
    opacity: 0;
    border-radius: 2em;
    box-shadow: a.$box-shadow-hover;
    transition: opacity .5s ease-in-out;
}

.cta__btn:hover {
    box-shadow: 0 50px 20px -25px a.$clr-secondary-light inset; 
    transition: box-shadow .5s ease-in-out;
}

.cta__btn:hover::after{
    opacity: 1;
    transition: opacity .5s ease-in-out;
```

### <a id="4.2">4.2 - Animation 2 : Coeur</a>

**Points d'intérêt :**

- Une seule icône dans le HTML
- Une icone en CSS car n'a pas de sens dans l'HTML
- Une animation en keyframes

**Extraits de code :**

```scss
/* _heart.scss */

.heart-container {
    height: 50px;
    height: 4em;
    width: 4em;
    @include a.flex-center();
}

.fa-heart {
    font-size: 1.5em;
    position: relative;
    cursor: pointer;
    color: inherit;
    transition: color .5s ease;
}

.fa-heart::after {
    @include a.fa-icon($content:a.$ico-heart);
    position: absolute;
    top: 0;
    left: calc(50% - 0.5em);
    background-image: linear-gradient(a.$clr-primary, 10%, a.$clr-secondary);
    background-clip: text;
    -webkit-background-clip: text;
    color: transparent;
    opacity: 0;
}

.fa-heart:hover {
    opacity: 1;
    color: a.$clr-secondary;
    transition: color .5s ease;
}

.fa-heart:hover::after {
    opacity: 1;
    @include a.animation-heartBeat(.75s);
}

/* _animations.scss */

@mixin animation-heartBeat($duration: 1s) {

    animation: heartBeat $duration ease-in-out;

    @keyframes heartBeat {
        0% {
            opacity: 0;
            transform: scale(0);
        }

        85% {
            opacity: 1;
            transform: scale(1.35);
        }

        100% {
            opacity: 1;
            transform: scale(1);
        }
    }
}
```

### <a id="4.3">4.3 - Animation 3 : entrée progressive des cartes</a>

**Points d'intérêt :**

- Animation appliquée sur un élément parent
- Boucle for pour faciliter le maintien / l'évolution de la page peu importe le nombre d'éléments
- Incrément du délai d'animation selon la position *n* de l'élément

**Extraits de code :**

```scss
.restaurant__section {
    background-color: a.$clr-grey1;
    display: flex;
    flex-direction: column;
    gap: 1em 0;
    padding: 1em;
    @include a.animation-slideUp();
    
    @media screen and (min-width: 800px) {
        width: 500px;
    }
}

@mixin animation-slideUp {

    @for $i from 1 through 5 {
        & .menu-item:nth-child(#{$i}n) {
            animation: slideUp .75s forwards ease;
            animation-delay: #{$i * 0.25}s;
        }
    }

    @keyframes slideUp {
        0% {
            transform: translateY(50px);
            opacity: 0;
        }
        100% {
            transform: translateY(0);
            opacity: 1;
        }
    }
}


```

### <a id="4.4">4.4 - Animation 4 : container vert + rotation checkmark</a>

**Points d'intérêt :**

- Overflow hidden sur container
- Technique utilisée : margin négative
- Ellipse sur texte tronqué
- 2 animations déclenchées au survol de l'élément parent

**Extraits de code :**

```scss

.menu-item {
    @include a.flex-center($justify: space-between, $wrap:nowrap);
    border-radius: 15px;
    height: 4.5em;
    background-color: white;
    box-shadow: 0 2px 1px rgba(0, 0, 0, 0.12);
    overflow: hidden;
    opacity: 0;
    max-width: 600px;
}

.menu-item__check {
    @include a.flex-center();
    background-color: a.$clr-tertiary;
    color: white;
    height: 100%;
    width: 80px;
    margin-right: -80px;
    transition: margin .5s ease-in-out;
    cursor: pointer;
}

.menu-item:hover .menu-item__check {
    margin-right: 0;
}

.menu-item:hover .fa-check-circle {
    transform: rotate(360deg);
    transition-delay: 2s;
    transition: transform .5s ease-in;
 }

.menu-item__description {
    width: 100%;
    overflow:hidden;
    white-space: nowrap;
    padding-left: 1em;
    h3 {
        margin-bottom: 0.5em;
    }
    h3, p {
        overflow: hidden;
        text-overflow: ellipsis;
        /* min-width: 0; */
    }
}
```

### <a id="4.5">4.5 - Animation 5 : Loader</a>

**Points d'intérêt :**

- Loader : deux éléments (`#loader` = container, `#loader-animation-svg` = SVG intégré dans l'index.html)
- SVG animé par éléments individuels (lettres logo) ou par groupe (chef + poële, bras + assiette, bras + cloche)
- Animation en keyframes dans ./components/_loader-animation.scss
- Container du loader géré dans ./components/_loader.scss
- Durée totale : ( animation SVG 2.5s ) + ( animation container .5s ) = 3s

**Extraits de code :**

```scss
#loader {
    position: absolute;
    background: white;
    height: 100vh;
    width: 100vw;
    top: 0;
    bottom:0;
    right: 0;
    left: 0;
    z-index: 2;
    display: flex;
    flex-wrap: nowrap;
    justify-content: center;
    align-items: flex-start;
    
    animation-name: loaderVanish;
    animation-delay: 3s;
    animation-duration: .5s;
    animation-fill-mode: forwards;

    @keyframes loaderVanish {
        from {
            opacity: 1;
        }
        to {
            opacity: 0; 
            z-index: -1;
        }
    }
}

#loader-animation-svg{
    position: absolute;
    background-color: transparent;

    @media screen and (min-aspect-ratio:1/1) {
        height: 100vh;    
    }
    
    @media screen and (max-aspect-ratio:1/1) {
        width: 100vw;    
    }

    @include a.loader-animation(2500ms);

}
```

[Retour à l'index](#i)
___

## <a id="5">5 - Points d'amélioration</a>

- **Git** : Manque de recul sur la structure du projet pour bien prévoir le versionning avec Git
- **Git** : Manque de pratique de `git rebase`

- **Sass** : : `_functions.scss` vide laissé dans le repo (et `@forward` dans `./abstracts/_index.scss`)
- **Sass** : `@mixin animation-heartBeat()` mixin inutile car utilisée une seule fois dans tout le projet.

```scss
.fa-heart:hover::after {
    opacity: 1;
    @include a.animation-heartBeat(.75s);
}

@mixin animation-heartBeat($duration: 1s) {

    animation: heartBeat $duration ease-in-out;

    @keyframes heartBeat {
        0% {
            opacity: 0;
            transform: scale(0);
        }

        85% {
            opacity: 1;
            transform: scale(1.35);
        }

        100% {
            opacity: 1;
            transform: scale(1);
        }
    }
```

aurait simplement pû être :

```scss
.fa-heart:hover::after {
    opacity: 1;
    animation: heartBeat $duration ease-in-out;
    
    @keyframes heartBeat {
            0% {
                opacity: 0;
                transform: scale(0);
            }

            85% {
                opacity: 1;
                transform: scale(1.35);
            }

            100% {
                opacity: 1;
                transform: scale(1);
            }
    }
```

[Retour à l'index](#i)
___

## <a id="6">6 - Q&A</a>

Question de l'évaluateur :

> Quel a été l'intérêt d'utiliser Sass plutôt que CSS ?

Réponse :

>- Organisation plus simple des éléments par fonction (styles uniques vs composants réutilisables)
>- Evite la répétition (ex : mixins, fonctions)
>- Imbrication (nesting)

Autres avantages non-cités :

- [Page web ideematic.com](https://www.ideematic.com/dictionnaire-digital/css-sass/)
- [Page web ionos.fr](https://www.ionos.fr/digitalguide/sites-internet/developpement-web/sass/)

[Retour à l'index](#i)
___

## <a id="7">7- Débrief</a>
Question à l'évaluateur :

> On nous a encouragé à utiliser la méthodologie BEM, est-ce qu'elle est indispensable dans un contexte pro ?

Réponse :

> Ca dépend de l'entreprise

-> Pas obligatoire mais il est nécessaire de la connaître pour s'adapter à l'équipe.
[Retour à l'index](#i)

## Références
[^1]: [How to animate box-shadow with silky smooth performance - Tobias Ahlin](https://tobiasahlin.com/blog/how-to-animate-box-shadow/)