---
id: 290
title: Internationalisation i18 des messages avec angular JS et Spring MVC
date: 2016-11-09T17:21:12+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=290
permalink: /internationalisation-messages-angular-js/
categories:
  - JAVA
---
Le message bundle de Spring MVC permet de bien gérer l&rsquo;internationalisation des pages JSP , mais ne peut pas gérer directement les messages dans angular JS (surtout pour les templates HTML utilisés dans angular ) .
  
Pour gérer l&rsquo;internationalisation coté angular on utilisera un module qui s&rsquo;appelle « angular-translate » , son avantage est sa capacité à s&rsquo;interfacer avec des systèmes tiers pour récupérer les messages surtout avec spring MVC .

pour pouvoir utiliser ce module il faut importer les librairies suivantes : 

<pre class="brush: jscript; title: ; notranslate" title="">&lt;script src="https://cdnjs.cloudflare.com/ajax/libs/angular-translate/2.13.0/angular-translate.js"&gt;&lt;/script&gt;
        &lt;script src="https://cdnjs.cloudflare.com/ajax/libs/angular-translate/2.13.0/angular-translate-storage-local/angular-translate-storage-local.min.js"&gt;&lt;/script&gt;
        &lt;script src="https://cdnjs.cloudflare.com/ajax/libs/angular-translate/2.13.0/angular-translate-storage-cookie/angular-translate-storage-cookie.min.js"&gt;&lt;/script&gt;
        &lt;script src="https://cdnjs.cloudflare.com/ajax/libs/angular-translate/2.13.0/angular-translate-loader-url/angular-translate-loader-url.min.js"&gt;&lt;/script&gt;
        &lt;script src="https://cdnjs.cloudflare.com/ajax/libs/angular-translate/2.13.0/angular-translate-loader-static-files/angular-translate-loader-static-files.min.js"&gt;&lt;/script&gt;
        &lt;script src="https://cdnjs.cloudflare.com/ajax/libs/angular-translate/2.13.0/angular-translate-loader-partial/angular-translate-loader-partial.min.js"&gt;&lt;/script&gt;
</pre>

ensuite il faut ajouter la configuration suivante dabs notre module application 

<pre class="brush: jscript; title: ; notranslate" title="">var appModule= angular.module("appModule", ['pascalprecht.translate']);
//config angular i18
iotDataVizModule.config(function ($translateProvider) {
	
    // synchronise with spring message bundle 
    $translateProvider.useUrlLoader('/gui/messageBundle');
    
    //set user local
    $translateProvider.uniformLanguageTag('bcp47').determinePreferredLanguage(function () {  	
    	  var preferredLangKey = moment.locale();
    	  console.log("preferredLangKey : " + preferredLangKey);
    	  return preferredLangKey;
    	});
    
    //security strategy
    $translateProvider.useSanitizeValueStrategy('escape');

});

</pre>

  * useUrlLoader : permet de sepécifier l&rsquo;url pour récupérer les messages , dans notre cas ca sera une méthode du controller Spring Mvc
  * determinePreferredLanguage : permet de sepécifier la langue à utiliser , dans notre cas ca sera la locale du browser 

la méthode du controller pour récupérer les messages avec spring mvc en fonction de la langue : 

<pre class="brush: java; title: ; notranslate" title="">@Inject
private SerializableResourceBundleMessageSource messageBundle;

    @RequestMapping(method = RequestMethod.GET , value="/gui/messageBundle")
    @ResponseBody
    public Properties list( @RequestParam String lang) {
    	return messageBundle.getAllProperties(new Locale(lang));
    }
</pre>

<pre class="brush: java; title: ; notranslate" title="">public class SerializableResourceBundleMessageSource extends ReloadableResourceBundleMessageSource{
	
	public Properties getAllProperties(Locale locale) {
        clearCacheIncludingAncestors();
        PropertiesHolder propertiesHolder = getMergedProperties(locale);
        Properties properties = propertiesHolder.getProperties();

        return properties;
    }
}
</pre>

Pour afficher les messages dans un template HTML : 

<pre class="brush: jscript; title: ; notranslate" title="">{{'message.code'| translate}}
</pre>

Et pour les récupérer dans un controller angular : 

<pre class="brush: jscript; title: ; notranslate" title="">appModule.controller('myController',['$scope','$translate'], function($scope , $translate){
			//i18 labels
			$translate(['message.code1' , 'message.code2'])
			.then(function (translations) {
			    $scope.message1 = translations['message.code1'];
			    $scope.message2= translations['message.code2'];
			  });
}]);

</pre>

Règle d&rsquo;internationalisation en spring MVC : 

&#8211; Pour bien gérer l’internationalisation avec Spring MVC il faut disposer d&rsquo;un fichier properties par langue .
    
chaque fichier doit être nommé comme suit : « basename »\_localisation.properties (ex : messages\_fr.properties , message_en.properties..)
  
en plus de ce fichier il faut avoir un fichier par défaut qui ne contient pas de suffixe localisation (ex : messages.properties )

par défaut spring gérera le choix du fichier selon la règle suivante :
  
&#8211; S&rsquo;il trouve le fichier correspondant à la langue , il prend ce fichier
  
&#8211; sinon si la langue de l&rsquo;utilisateur ne correspond à aucun fichier , c&rsquo;est la langue par défaut paramétrée dans le local Resolver de spring
  
&#8211; Sinon c&rsquo;est la langue du système (langue serveur ou installé l&rsquo;application ) qui est prise en compte .
  
&#8211; sinon si aucune langue trouvé ne correspond aux fichiers , c&rsquo;est le fichier par défaut sans extension de localisation qui est pris en compte (messages.properties )
  
.