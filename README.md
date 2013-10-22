# HTML/CSS Style Guide

## Règles générales

 - Utiliser des indentations de 2 espaces.  
 - Utiliser des minuscules pour les noms d'éléments, attributs, sélecteurs, valeurs, etc.
 - Utiliser des tirets pour délimiter les noms de classe ou d'attributs.
 - Utiliser des guillemets doubles.
 - Utiliser l'anglais de préférence pour les noms de classes ou de mixins.
 

## CSS/Sass

### Découpage des fichiers

Préférer découper les feuilles de styles en petits fichiers (ex. : `_partial.scss`), si possible un par composant/module, et importer le tout dans un fichier `application.scss` ou `front.scss`.  
Importer d'abord les librairies (ex. : compass ou bourbon), puis les `mixins` et les `variables`.  
Importer ensuite le reset, les déclarations de polices (`@font-face`), les éléments de base (`body`, `h1`, `p`), le layout (mise en page générale), puis les composants/modules. Vient ensuite les thèmes et les helpers.

    @import "compass",
    	    "compass/reset";
    
	@import "front/variables",

      		"front/utils/hdpi",
      		"front/utils/rem",
      		"front/utils/mixins",
		
      		"front/fonts",
      		"front/base",
      		"front/layout",

      		"front/modules/buttons",
      		"front/modules/forms",
      		"front/modules/blocks",
      
      		"front/helpers",
      		"front/themes";      		 


### Indentation & mise en page

Passer à la ligne pour chaque bloc puis indenter son contenu.  
Indenter les blocs en fonction de la profondeur des éléments HTML correspondants.  
Passer à la ligne après chaque sélecteur et chaque déclaration, laisser un espace entre la `propriété:` et sa `valeur` et terminer chaque déclaration par un point-virgule.  

    [selecteur],
    [selecteur] {
      [propriété]: [valeur];
      [propriété]: [valeur];
    }
    
      [selecteur] {
        [propriété]: [valeur];
      }     

Dans le cas de déclarations simples (ie. d'une seule propriété), il est possible de simplifier la notation : 

    [selecteur] { [propriété]: [valeur]; }


### Convention de nommage

Délimiter les noms de classe par des tirets : `.ma-classe` et non `.ma_classe` ou `.MaClasse`.  

Utiliser dès que nécessaire la notation BEM (Block, Élément, Modifieur) :

    .block {}
    .block--modifieur {}
    .block__element {}
    .block__element--modifieur {}
    
`.block` représente la racine du composant/module, `.block__element` son descendant/enfant.  
`.block--modifieur` et `.block__element--modifieur` représentent un état ou une version différente de `.block` et de `.block__element`.


### Hooks JS

Utiliser des préfixes de classes `.js-` pour cibler les éléments via JavaScript. **Ces classes ne doivent pas apparaitre dans les feuilles de style.**

Exemple : 

    <ul class="tabs js-tabs">


### Ordre des déclarations

Classer les déclarations par ordre alphabétique en les regroupant par pertinence (ex. : `position`/`left`/`top`, `height`/`width`, `margin`/`padding`).  
Grouper les implémentations propriétaires (préfixes), en écrivant toujours la version officielle en dernier.  

Si utilisation d'un préprocesseur, regrouper les `@extend` en début de déclaration et les sélecteurs imbriqués (`& > img`, `&:hover` ou `&:before`) à la fin.

Exemple : 

    .block {
      @extend %block, .clearfix;

      background: blue;
      color: green;

      @include border-radius(10px);
      border: 1px solid red;
      @include box-shadow(0 0 10px rgba(#000, .5));
      @include box-sizing(border-box);
      
      content: "";

      clear: both;
      display: block;
      float: left;

      font-size: 1em;
      font-weight: bold;
      letter-spacing: 2px;
      line-height: 1.8;
      
      list-style: none;

      height: 100px;
      width: 100px;

      margin: 0;
      padding: 0;

      overflow: hidden;
      
      position: absolute;
      left: 0;
      top: 0;
      z-index: 2;

      text-align: center;
      text-decoration: underline;
      text-transform: uppercase;
      @include text-shadow(0 1px rgba(#000, .8));
    }


### Sélecteurs

Essayer de réduire au maximum la spécificité des sélecteurs afin de maximiser la portabilité et les performances.  
Si utilisation d'un préprocesseur, attention à l'imbrication des blocs, qui va générer des sélecteurs trop spécifiques. Essayer de se limiter à trois imbrications au maximum.

Éviter au possible de typer les sélecteurs et d'utiliser des IDs afin que le code soit réutilisable et maintenable.

Par exemple, le code suivant :

    #content div.block {
      display: block;
      
      h1.block-title {
        color: red;
      }
      
      div.block-content {
        color: blue;
        
        a{
          color: green;
        }
      }
    }

Va générer :

    #content div.block {
      display: block;
    }
    
    #content div.block h1.block-title {
      color: red;
    }
    #content div.block div.block-content {
      color: blue;
    }
    
    #content div.block div.block-content a{
      color: green;
    }

Recommandé : 

    .block {
        display: block;
    }
    
      .block-title {
        color: red;
      }
      
      .block-content {
        color: blue;
        
        a{
          color: green;
        }
      }      				


### Unités & valeurs

Ne pas mettre d'unité quand la valeur est `0` et ne pas mettre de `0` avant les valeurs entre `-1` et `1`, ex. : `letter-spacing: -.025em;`.  
Pour les couleurs, utiliser des minuscules et la notation hexadécimale à 3 chiffres dès que possible, ex. : `#aaa`. 
Préférer les guillemets doubles aux simples (interpolation de variable possible dans les guillemets doubles avec `#{$variable}` via Sass).

Utiliser un mélange de `px`, `em`, `rem` et `%` pour les dimensions.  
Utiliser `rem` avec un fallback en `px` pour les anciens navigateurs ([mixin](https://github.com/pierreburel/rem)).  
Ne pas préciser d'unité pour la propriété `line-height`.

Exemple : 

    .block {
      color: #f00;
      content: "Test";
      font-family: Arial, sans-serif;
      font-size: 1.6rem;
      font-size: 16px;
      line-height: 1.2;            
      margin: 0 5% 1.2em 5%;
      opacity: .5;
    }


### Shorthands

Attention à l'utilisation des shorthands : `background: red;` défini un fond uni de couleur rouge, et écrase ainsi d'éventuelles déclarations précédentes (`background-image` par exemple).  
De même pour `margin` et `padding` : spécifier une marge droite avec `margin: 0 10px 0 0;` écrasera les autres marges éventuelles. Lui préférer `margin-right: 10px;`


### !important

Réserver l'utilisation d'`!important` pour les helpers, et non pour résoudre des problèmes de sélecteurs sur-qualifiés (voir la partie sur les sélecteurs). 
    
    
### Commentaires

Au minimum, utiliser des commentaires simples quand le code nécessite des explications.  

Exemple : 

    // Commentaire simple en Scss
    /* Commentaire simple en CSS */
    
Il est possible d'utiliser des commentaires avancés dans le cas d'un projet conséquent : table des matières, titres, etc. 

Ajouter une table des matières permettant de visualiser le contenu de la feuille de style.

Exemple : 

    /*------------------------------------------------------------------
    [Table des matières]

    1. Body
      2. Header / #header
        2.1. Navigation / #navbar
      3. Content / #content
        3.1. Left column / #leftcolumn
        3.2. Right column / #rightcolumn
        3.3. Sidebar / #sidebar
          3.3.1. RSS / #rss
          3.3.2. Search / #search
          3.3.3. Boxes / .box
          3.3.4. Sideblog / #sideblog
          3.3.5. Advertisements / .ads
      4. Footer / #footer
    -------------------------------------------------------------------*/

Toujours dans le but de cibler rapidement les propriétés, on rajoutera une en-tête au-dessus d'un ensemble de propriétés.

Exemple CSS :
    
    /* ==========================================================================
       Bloc de commentaire de section
       ========================================================================== */
    
    /* Bloc de commentaire de sous-section
       ========================================================================== */
    
    /*
     * Groupe de bloc de commentaires.
     * Parfait pour les explications sur plusieurs lignes et la documentation.
     */
    
    /* Commentaire simple */

Exemple SCSS :

    // ==========================================================================
    // Bloc de commentaire de section
    // ==========================================================================
    
    // Bloc de commentaire de sous-section
    // ==========================================================================
    
    //
    // Groupe de bloc de commentaires.
    // Parfait pour les explications sur plusieurs lignes
    // et la documentation

    // Commentaire simple


====


## HTML

### Doctype

Utiliser un doctype HTML5 dès que possible :

    <!DOCTYPE html>
    
Équivalent Haml :

     !!5
     
### Commentaires conditionnels

Utiliser des commentaires conditionnels pour ajouter des classes dans l'élément `<html>` pour cibler les anciennes versions d'Internet Explorer, ainsi que les utilisateurs ayant JavaScript désactivé :

    <!--[if lt IE 9]><html lang="fr" class="no-js lt-ie9"><![endif]-->
    <!--[if IE 9]><html lang="fr" class="no-js ie9"><![endif]-->
    <!--[if (gt IE 9)|!(IE)]><!--><html lang="fr" class="no-js"><!--<![endif]-->
    
Utiliser [Modernizr](http://modernizr.com) pour ajouter d'autres classes pour détecter le support de technologies HTML, CSS et JavaScript.


### Syntaxe et validité

Essayer au maximum d'utiliser la syntaxe XHTML et éviter les facilités accordées par HTML5 (balises fermantes optionnelles, valeurs d'attributs sans guillemets, etc).  
Ne pas omettre l'attribut `alt` des éléments `<img/>` : le laisser à vide pour les images purement décoratives.

Non recommandé :

    <ul class=products>
      <li class="first product">Test
      <li>Test
      <li class="last product">Test
    <img src="image.jpg">

Recommandé :

    <ul class="products">
      <li class="first product">Test</li>
      <li>Test</li>
      <li class="last product">Test</li>
    </ul>
    <img src="image.jpg" alt=""/>


### Sémantique

Utiliser les éléments HTML de manière sémantique et séparer la structure de la présentation.

Non recommandé :

    <div class="MonTitre couleur_rouge">Titre</div>
    <center class="monTexte couleur_bleu">Mon texte</center>

Recommandé :

    <div class="block">
      <h1 class="block__title">Titre</h1>
      <div class="block__content">
        <p>Mon texte</p>
      </div>
    </div>