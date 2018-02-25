---
id: 278
title: Récupérer le trimistre en cours en java 8
date: 2016-10-24T16:13:05+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=278
permalink: /recuperer-trimistre-cours-java-8/
categories:
  - JAVA
---
Supposons que T1 ,T2 , T3 et T4 représente les 4 trimestres de l&rsquo;année .
  
on crée une enumération qui représentera les trimistres 

<pre class="brush: java; title: ; notranslate" title="">public enum Quarter {	
	T1,T2,T3,T4
}

	public static Quarter getQuarterCode(LocalDate date){
		
		int month = date.get(ChronoField.MONTH_OF_YEAR);
		if (month &lt;= 3) {
			return   Quarter.T1;
        } else if (month &lt;= 6) {
        	return  Quarter.T2;
        } else if (month &lt;= 9) {
        	return Quarter.T3;
        } else {
        	return Quarter.T4;
        }
		
	}

public static Quarter getCurrentQuarterCode(){
		
		return getQuarterCode(LocalDate.now());
}
		


</pre>