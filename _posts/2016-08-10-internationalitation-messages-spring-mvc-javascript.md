---
id: 218
title: Internationalitation des messages dans Javascript en utilisant Spring MVC
date: 2016-08-10T11:17:14+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=218
permalink: /internationalitation-messages-spring-mvc-javascript/
categories:
  - JAVA
---
&nbsp;

L&rsquo;idée de cet article est d&rsquo;internationaliser les messages dans javascript en utilisant le mécanisme d&rsquo;internalisation de Spring MVC avec la balise <spring:message />.

Pour ce faire on va créer une méthode dans le Controller Spring qui récupère tous les codes messages du fichier properties

<pre class="brush: java; title: ; notranslate" title="">@RequestMapping(value="messages.js")
 public ModelAndView mesages(HttpServletRequest request)  {
 
 // Retrieve the locale of the User
 Locale locale = RequestContextUtils.getLocale(request);
 
 // Use the path to your bundle
 ResourceBundle bundle = ResourceBundle.getBundle("messages/messagesProperties", locale); 
 
 // Call the messages.jsp view
 return new ModelAndView("messages", "keys", bundle.getKeys());

}

</pre>

Cela suppose que mes fichiers  messagesProperties.properties se trouvent dans le classPath (Généralement  dans le dossier src/main/resources )

Cette méthode sera appelée au chargement de chaque page auquelle on veut accèder au message  .

<pre class="brush: java; title: ; notranslate" title="">&lt;script type="text/javascript" src="${pageContext.request.contextPath}/messages.js"&gt; &lt;/script&gt;

</pre>

Les codes messages seront intégrés dans une jsp qui va essayer de récupérer chaque code message et sa valeur et le mettre dans une variable  Javascript de type tableau

&nbsp;

Le code de la JSP (messages.jsp ) est :

<pre class="brush: xml; title: ; notranslate" title="">&lt;%@page contentType="text/javascript" pageEncoding="UTF-8"%&gt;
&lt;%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%&gt;
&lt;%@taglib prefix="spring" uri="http://www.springframework.org/tags"%&gt;

var messages = new Array();

&lt;c:forEach var="key" items="${keys}"&gt;
messages["&lt;spring:message text='${key}' javaScriptEscape='true'/&gt;"] = 
"&lt;spring:message code='${key}' javaScriptEscape='true' /&gt;";
&lt;/c:forEach&gt;


</pre>

&nbsp;

on peut accèder au messages dans javascript  en utilisant la variable messages .

<pre class="brush: jscript; title: ; notranslate" title="">alert(messages['projet.message.code']; 
</pre>

avec « projet.message.code » est le code du message qu&rsquo;on veut afficher .

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;