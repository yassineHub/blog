---
id: 153
title: Ordonner les jours de la semaine en commancant par aujourduit en java
date: 2016-05-30T14:04:57+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=153
permalink: /ordonner-jours-de-semaine-commancant-aujourduit-java/
categories:
  - JAVA
---
Pour ordonner les dates du jour en commancant par la date du jour , on va se baser sur l&rsquo;ordre retourné par la méthode getValue() de l&rsquo;objet DayOfWeek .
  
dans cet objet , MONDAY a la valeur 1 , TUESDAY la valeur 2 , &#8230; et ainsi de suite jusqu&rsquo;à SUNDAY qui a la valeur 7 .
  
Donc pour ce faire on va essayer d&rsquo;implémenter un algorithme qui va attribuer un poids à chaque jour de la semaine de tel facon à avoir le poids le plus faible (valeur 0) pour le jour de la semaine qui corrrespond à aujourd’hui .
  
Dans un premier temps on va essayer de créer un objet schedule qui contient deux attributs , un attribut de type DayOfWeek et un autre weight pour représenter le poids
  
`<br />
		LocalDate today = LocalDate.now();<br />
		DayOfWeek DayOfWeekOfToday = today.getDayOfWeek();<br />
		int todayOrder = DayOfWeekOfToday.getValue();<br />
` 
  
Ensuite on va itérer sur l&rsquo;enum DayOfWeek pour ordonner les jours de la semaine
  
`<br />
for(DayOfWeek dayOfweek: DayOfWeek.values()) {<br />
int weight = 0 ;<br />
if(dayOfweek.getValue() >= todayOrder){<br />
	    			weight = (8- todayOrder + dayOfweek.getValue()) % 8 ;<br />
	    		}else{<br />
	    			weight = (8- todayOrder + dayOfweek.getValue() + 7 ) % 8 ;<br />
	    		}<br />
                        schedule.setDay(dayOfweek);<br />
	    		schedule.setWeight(weight);<br />
}<br />
` 

après pour ordonner , ca devient plus facile , on utilisant une TreeList avec un comparator basé sur les poids des jours
  
`<br />
    	TreeList<Schedule> list= new TreeList<>((o1,o2)->o1.getWeight().compareTo(o2.getWeight()));    </p>
<p>`