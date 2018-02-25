---
id: 127
title: Le patron de conception Module
date: 2016-05-07T15:52:54+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=127
permalink: /pattern-module-javascript/
categories:
  - JavaScript
---
Ce pattern a pour but de structurer et d&rsquo;organiser l&rsquo;architecture de code javascript sous forme de module .

Un module en javascript peut etre crée simplement en créant une variable globale à la quelle on affecte une méthode anonyme appelée imédiatement (function self invoking )

<pre class="brush: jscript; title: ; notranslate" title="">var module =;(function(){...})();

</pre>

La structure du module

<pre class="brush: jscript; title: ; notranslate" title="">var MODULE = (function(){

  "use strict"; //  dit au navigateur que tout ce qui se trouve dans cette fonction devra être traité par un parseur strict qui vous signalera plus d’erreurs de code

  var instance = {};
  var attributPrive = "je suis attribut privé";

  instance.attributPublic = "je suis attribut public ";

  function methodePrivee() {
  console.log('Méthode privé accesible juste à l'interieur du module');
  }

  instance.methodePublique = function(){
  console.log('Méthode publique accessible depuis l'extérieur du module ');
  };

  return instance;
})();
</pre>

MODULE.methodePublique();// affiche  » Méthode publique accessible depuis l&rsquo;extérieur du module  »

<div>
  MODULE. methodePrivee() // Affichera une erreur : methodePrivee is not defined !!
</div>

<div>
  Dans la variable local « instance » on met tous les éléments publique du module , c&rsquo;est à dire les élement auquel on veut y accéder depuis l&rsquo;ectérieur du module , les autres éléments (variable local et fonction privés ) sont inaccessible depuis l&rsquo;extérieur .
</div>

<div>
  La variable module contiendras donc le contenu de la variable instance
</div>

<div>
  Il est recommendé créer un module par fonctionnalité .
</div>

<div>
</div>

<div>
  <b>Sous modules</b>
</div>

<div>
  <span style="text-decoration: underline;">Le module principale : </span>
</div>

<div>
  <pre class="brush: jscript; title: ; notranslate" title="">
var ModulePrincipale =  (function(){

 /* ici on peut faire appel aux fonctions du sous module */
})();
</pre>
</div>

<div>
  Le sous  module:
</div>

<div>
  <pre class="brush: jscript; title: ; notranslate" title="">
ModulePrincipale.SousModule = (function(){   /* sous module enfant */
})();
</pre>
</div>

<div>
  <strong style="color: #111111; font-family: Arimo, Verdana, Arial; font-size: 0.95em; line-height: 1.6em; background-color: #ffffff;">Ce n&rsquo;est pas de l&rsquo;héritage !</strong> mais plutot aléger le module en externalisant certains fonctionalité dans d&rsquo;autres sous modules , ca permet l&rsquo;aléger un gros module qui contient énormenement de code .
</div>

<div>
  <div>
    Dans ce cas le sous module n&rsquo;a pas acces au modue parent , c&rsquo;est plutot ce dernier qui appelle certain fonction du sous module .
  </div>
  
  <div>
    Attention à ne pas déclarer un sous-module <strong>AVANT</strong> un module. Le module parent (la variable) serait non défini.
  </div>
</div>

<div>
</div>

<div>
  <span style="text-decoration: underline;"><strong>Un module globale pour notre application</strong></span>
</div>

<div>
  <div>
    L&rsquo;idée est de créer un module parent qui englobera l&rsquo;ensemble des modules enfants.
  </div>
  
  <div>
    <p>
      Le chargement de vos fichiers pouvant intervenir dans un ordre aléatoire, il faut faire attention à bien vérifier l&rsquo;existence du parent avant de tenter d&rsquo;y ajouter un enfant. Voilà comment procéder pour un parent se nommant AppModule :
    </p>
    
    <pre class="brush: jscript; title: ; notranslate" title="">

var AppModule = AppModule    || {};

</pre>
    
    <p>
      <span style="color: #424242; font-family: Merriweather, 'Liberation Serif', 'Times New Roman', Times, Georgia, FreeSerif, serif;">il faut mettre cette ligne de code  dans le fichier de module à chaque fois qu&rsquo;on veut créer un module   . cette instruction signifie qu&rsquo;on essaie de récupérer dans un premier temps notre module et si il n&rsquo;existe pas on le créer . il permet d’empêcher d’écraser </span> AppModule   si celui-ci existe déjà et qu&rsquo;il contient d&rsquo;autres modules qui ont été chargés avant.
    </p>
  </div>
  
  <p>
    <strong><span style="text-decoration: underline;"><a href="https://zestedesavoir.com/tutoriels/358/module-pattern-en-javascript/#6-11124_extensions-de-modules">Extensions de modules</a></span></strong>
  </p>
  
  <p>
    Le but est de pouvoir ajouter des fonctionnalités à un module sans modifier le code du module existant .
  </p>
  
  <pre class="brush: jscript; title: ; notranslate" title="">
var AppModule = AppModule || {};

AppModule.Module= (function(instance){

    /* toutes les fonctions du Module*/

    return instance;
})( AppModule.Module|| {});
</pre>
</div>

<div>
</div>

<div>
  Dans un premier temps on essaie de récupérer notre module globale <strong>AppModule ou le créer si il n&rsquo;existe pas .</strong>
</div>

<div>
  on passe en paramètre du module la référence du module initial , il faut faire attention à ne par recréer la variable instance , pour ne pas écraser celle qui a été passé en paramètre .
</div>

<div>
</div>

##### <span style="text-decoration: underline;">Définir des options paramètrables d&rsquo;un module</span>

On peut définir des options avec des valeurs par défaut qu&rsquo;on peut surcharger avec une méthode d&rsquo;initialisation :

<pre class="brush: jscript; title: ; notranslate" title="">var MODULE = (function(){


  var instance = {};

 //options with defaults values 
 var settings = {
 defaultsIndexColors : { black : '#000000' , red : '#ff0000' , orange : '#ff8000' , yellow : '#FFE436' , green :'#006400' }
 }

 //to init or overwide setting
 instance.init = function (options){
 settings = $.extend(true, settings, options);
 console.log( settings);
 }

  return instance;
})();
</pre>

<div>
</div>