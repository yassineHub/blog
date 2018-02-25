---
id: 169
title: Limite des selecteurs css sous IE
date: 2016-06-15T14:37:43+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=169
permalink: /limite-selecteurs-css-ie/
categories:
  - JAVA
---
Problème ?
  
J&rsquo;ai rencontré un problème de CSS sur un projet utilisant la biblithèque métro UI , En fait le projet fonctionne bien sous chrome mais pas IE .
  
En investiguant j&rsquo;ai constaté en utilisant la console de développement de IE (F12) qu&rsquo;une partie du code css n&rsquo;a pas été pris en compte par mon navigateur IE9 .

Cause ?

En fait certains version de IE ont des limitations en terme du nombre de règles et de relecteurs qu&rsquo;on peut déclarer dans un ficher css ou une page html, dépassé ce nombre le navigateur ignore sans généré d&rsquo;alerte ou erreur le code qui dépasse la limitation .
  
Les limitations sont :
  
&#8211; 31 tags style à l&rsquo;intérieur de la balise 

&#8211; 4095 sélecteurs à l&rsquo;intérieur d&rsquo;un fichier css
  
&#8211; 3 niveau utilisant l&rsquo;annotation @import

Les navigateurs qui ont ces limitations sont : IE6 jusqu&rsquo;à IE9

Solution 

Ce problème arrive souvent quand on utilise des compilateurs de CSS comme « LESS » pour avoir un seul fichier contenant l&rsquo;ensemble du code CSS .
  
Avoir un seul fichier nous permet de réduire le nombre d&rsquo;appelle http qui se produit lors du chargement de la page . mais dans le cas de gros projets , le fichier a une taille plus importante et du coup on dépasse les limitations de IE .

La solution est d&rsquo;essayer au mieux de créer plusieurs fichier css au lieu d&rsquo;un seul .
  
Il y a sur le net pas mal d&rsquo;outils qui permettent de couper un seul fichier en plusieurs en respectant les limitations , parmi ces outils , moi j&rsquo;ai utilisé Bless

Comment savoir si on a dépassé les limitations ?

L&rsquo;outil dont je vous ai parlé en haut bless permet de calculer le nombre de selecteur dans un fichier css .
  
Sinon Voici un script javascript qu&rsquo;on peut passer dans la console du navigateur pour afficher le nombre de selecteurs des fichiers css d&rsquo;une page .

<pre class="brush: jscript; title: ; notranslate" title="">function countCSSRules() {
    var results = '',
        log = '';
    if (!document.styleSheets) {
        return;
    }
    for (var i = 0; i &lt; document.styleSheets.length; i++) {
        countSheet(document.styleSheets[i]);
    }
    function countSheet(sheet) {
        if (sheet && sheet.cssRules) {
            var count = countSelectors(sheet);

            log += '\nFile: ' + (sheet.href ? sheet.href : 'inline &lt;style&gt; tag');
            log += '\nRules: ' + sheet.cssRules.length;
            log += '\nSelectors: ' + count;
            log += '\n--------------------------';
            if (count &gt;= 4096) {
                results += '\n********************************\nWARNING:\n There are ' + count + ' CSS rules in the stylesheet ' + sheet.href + ' - IE will ignore the last ' + (count - 4096) + ' rules!\n';
            }
        }
    }
    function countSelectors(group) {
        var count = 0
        for (var j = 0, l = group.cssRules.length; j &lt; l; j++) {
            var rule = group.cssRules[j];
            if (rule instanceof CSSImportRule) {
                countSheet(rule.styleSheet);
            }
            if (rule instanceof CSSMediaRule) {
                count += countSelectors(rule);
            }
            if( !rule.selectorText ) {
                continue;
            }
            count += rule.selectorText.split(',').length;
        }
        return count;
    }

    console.log(log);
    console.log(results);
};
countCSSRules();
</pre>