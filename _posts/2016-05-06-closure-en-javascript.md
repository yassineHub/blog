---
id: 123
title: Closure en Javascript
date: 2016-05-06T23:18:12+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=123
permalink: /closure-en-javascript/
categories:
  - JavaScript
---
### <span style="text-decoration: underline;">Portée des variables</span>

<div>
  On a deux type de portée :
</div>

<div>
  &#8211; Portée  locale : Une variable défini à l&rsquo;interieur d&rsquo;une fonction avec le mot clé « var »  n&rsquo;est disponible qu&rsquo;à l&rsquo;intérieur de cette fonction
</div>

<div>
  &#8211; Portée globale :   Une variable est considérée comme globale si il n&rsquo;est pas définie à l&rsquo;intérieur d&rsquo;une méthode (on s&rsquo;en fiche s&rsquo;il est précédé par le mot clé « var » ) ou bien s&rsquo;il est déclaré à l&rsquo;intérieur d&rsquo;une méthode sans le mot clé « var »
</div>

<div>
</div>

<div>
  Tous les scripts et les fonctions dans une page web peuvent accèder à une variable global
</div>

<div>
</div>

<div>
  Remarque !!
</div>

<div>
</div>

<div>
  <span style="color: rgba(0, 0, 0, 0.85098); font-family: 'Source Sans Pro', sans-serif;">Une variable local déclaré à l&rsquo;interieur d&rsquo;une fonction (avec le mot clé var )  peut avoir le meme nom qu&rsquo;une autre variable global , c&rsquo;est la variable local qui sera prioritaire et prise en compte à l’intérieur de cette fonction</span>
</div>

<div>
  les arguments  d&rsquo;une fonction sont considérés comme variable local
</div>

### 

### <span style="text-decoration: underline;">Le mot clé var : </span>

<div>
   en gros le mot clé var est utilisé pour limiter le scope d&rsquo;une variable à la fonction dans laquelle elle est déclaré , sinon , si on déclare une variable sans « var » elle est automatiquement considéré comme globale
</div>

<div>
</div>

<div>
  <span style="text-decoration: underline;">Durée de vie des variable</span>
</div>

<div>
  les variables locales sont écrasé quand on quitte une fonction
</div>

<div>
  les variable globales sont écrasé quand on ferme la page web
</div>

<div>
</div>

<div>
  <span style="text-decoration: underline;">Les variables globales en HTML</span>
</div>

<div>
  en JS  , le scope  global est tous l&rsquo;envirenement de javascript
</div>

<div>
  en HTML , le scope global est l&rsquo;objet « windows » , tous les variables globales JS appartient à l&rsquo;objet window
</div>

<div>
</div>

<div>
  <pre class="brush: jscript; title: ; notranslate" title="">
// code here can use window.carName
function myFunction() {
    carName = "Volvo";
}
</pre>
</div>

<div>
</div>

<div>
  vos variables globales (ou fonctions) peuvent écraser des variables de de l&rsquo;objet window (ou fonctions). Toute fonction, y compris l&rsquo;objet window , peuvent écraser vos variables et fonctions globales.
</div>

<div>
</div>

<div>
  <h3>
    <span style="text-decoration: underline;">Portée des fonctions</span>
  </h3>
</div>

<div>
  &#8211; une fonction déclaré à intérieure d&rsquo;une autre fonction a une porté local
</div>

<div>
</div>

<div>
  <div>
    <h3>
      <span style="text-decoration: underline;">Les arguments des fonctions</span>
    </h3>
  </div>
  
  <div>
    &#8211; la valeur qu&rsquo;on affecte à un argument d&rsquo;une fonction est copié directement dans ce dernier
  </div>
</div>

<div>
  <pre><pre class="brush: jscript; title: ; notranslate" title="">
function afficherArgument(arg){
arg ="toto"; 
alert(arg); 
}
</pre>
</div>


<h3>
  Ce code affichera toujours toto meme car la valeur de l&rsquo;arguement a été modifié
</h3>


<h3>
  Exemple
</h3>


<div>
  Un exemple pour résumer tous ces notions :
</div>


<div>
  
</div>


<div>
  <pre><pre class="brush: jscript; title: ; notranslate" title="">
var variable_global_1 = "variableGlobal_1";

variable_global_2 = "variableGlobal_2" ;

function ma_fonction(argPrimitif){
	var variable_local_1 = "variable_local_1";
	variabe_gobal_3 = "variabe_gobal_3"
	var variable_global_1 ="variable local !!!!"
	argPrimitif = 5 ;
	

function methode_prive(){//...}

}
</pre>
</div>


<div>
  * variable_global_1   est global car n&rsquo;est définie à l&rsquo;intérieur de aucune fonction
</div>


<div>
  * variable_global_2  est global car déclarée sans le mot clé var donc automatiquement considéré comme global
</div>


<div>
  * variable_local_1 est local car déclaré à l&rsquo;intérieur d&rsquo;une fonction avec le mot clé var
</div>


<div>
  * variable_global_3 est considérée comme global car déclaré sans le mot clé var
</div>


<div>
  * la valeur 5 est copié dans l&rsquo;argument argPrimitif
</div>


<div>
  
</div>


<h3>
  <span style="text-decoration: underline;">les variables globales sont à éviter  </span>
</h3>


<div>
  Les variable globales à éviter car :
</div>


<div>
  &#8211; elles peuvent être modifié ou écrasé par d&rsquo;autre script
</div>


<div>
  &#8211; plus ils sont loin , plus on perd en performance car le temps d&rsquo;accès est lent
</div>


<div>
  
</div>


<div>
  Donc l&rsquo;idée pour limiter l&rsquo;utilisation des variables globales est d&rsquo;utiliser le <strong>pattern module</strong>  , il consiste à mettre tous le code à l&rsquo;intérieur de fonctions qui retournent des objet contenant les fonctions dont on a besoin d&rsquo;y accéder depuis l&rsquo;éxtérieur de notre module et assigner cette valeur retourné à une seule  variable global . en quelque sorte cette variable globale sera le nom et l&rsquo;identifiant de notre module
</div>


<div>
  
</div>


<div>
  <span style="text-decoration: underline;"><strong> Fonctions anonymes </strong></span>
</div>


<div>
  En Javascript on peut créer une fonction sans lui donner de nom. C’est ce qu’on appelle une <a href="http://sametmax.com/fonctions-anonymes-en-python-ou-lambda/">fonction anonyme</a>
</div>


<div>
  <div>
    Une fontion  anonyme  appelée immédiatement ( <strong>self-invoking function ) : c&rsquo;est le prinipe de déclaration de fonction qui sont appelé exécuté au moment de la déclaration , pour cela  :</strong>
  </div>
  
  
  <div>
    <strong>* on entoure la méthode par des parenthèse , ce qui nous permet de récupérer sa référence </strong>
  </div>
  
  
  <div>
    <b>* puis on ajoute deux parenthèse pour appeler cette fonction , </b>
  </div>
  
  
  <div>
    <pre class="brush: java; title: ; notranslate" title="">(function(){...})();</pre>
    
  </div>
  
  
  <div>
    du coup une fois la fonction est appelé , sa réference sera perdu et on ne peut plus l&rsquo;appeler une deuxièmre fois .
  </div>
  
</div>


<div>
  
</div>


<div>
  
</div>


<div>
  
</div>