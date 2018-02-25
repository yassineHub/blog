---
id: 226
title: Gestion des dates avec JPA
date: 2016-08-16T14:29:30+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=226
permalink: /lannotation-jpa-temporal/
categories:
  - JAVA
---
### <span style="text-decoration: underline;"><strong>Avant Java 8</strong></span>

Dans un des projet , j&rsquo;ai rencontré l&rsquo;erreur ci dessous :

<pre class="brush: java; title: ; notranslate" title="">Timestamp format must be yyyy-mm-dd hh:mm:ss.fffffffff exception

</pre>

&nbsp;

Et en déboguant j&rsquo;ai constaté que la conversion des dates ne se fait pas bien entre les types de base de données Sql server et les types Java  .

Pour résoudre ce problème de conversion , La JPA propose l&rsquo;annotation @Temporal qui permet de spécifier le type exacte qui est stocké en base de données et qui sera utilisé pour la conversion .

@Temporal est une annotation JPA qui est utilisé que pour les types java.util.Date ou  java.util.Calendar . Cette annotation est disponible depuis la  JPA 1.0

En fait le problème ne se pose pas quand la classe java déclare des champs avec les types java.sql.Date ou java.sql.Time . mais plutot c&rsquo;est java.util.Date et  java.util.Calendar

qui posent problème.

@Temporal a 3 options :

  * TemporalType.DATE
  * TemporalType.TIME
  * TemporalType.TIMESTAMP

### <span style="text-decoration: underline;"><strong>Après Java 8</strong></span>

Avec Java 8 on utilise plutot LocalDate et LocalDateTime au lieu de java.util.Date

Le problème est que la java 8 n&rsquo;est pas supporté par les version antérieur ou égale à JPA2.1 vu que la java 8 vest sortie  après la release JPA2.1

Pour ce faire on va utiliser les converter de la JPA pour mapper les champs Les type de java 8 avec les types de données en bases de données .

Pour convertir Les LocalDate  :

<pre class="brush: java; title: ; notranslate" title="">import java.sql.Date;
import java.time.LocalDate;
import javax.persistence.AttributeConverter;
import javax.persistence.Converter;

@Converter(autoApply = true)
public class LocalDateAttributeConverter 
implements AttributeConverter&amp;lt;LocalDate, Date&amp;gt; {
	
    @Override
    public Date convertToDatabaseColumn(LocalDate locDate) {
    	return (locDate == null ? null : Date.valueOf(locDate));
    }

    @Override
    public LocalDate convertToEntityAttribute(Date sqlDate) {
    	return (sqlDate == null ? null : sqlDate.toLocalDate());
    }
}
</pre>

Attention il faut utiliser les type java.sql.Date et non pas java.util.Date !!

la propriété autoApply si elle est mise à true , permet de dire que cette conversion doit etre appliqué à toute les entités .

Pour que cette propriété autoApply soit détecté , la classe du converter doit etre dans le meme package que les entités ou dans des sous packages du package des entités .

Si la valeur de autoApply est false qui est la valeur par défaut , il faut spécifier le convertisseur pour chaque champ auquel on veut appliquer la conversion .

<pre class="brush: java; title: ; notranslate" title="">@Column
@Convert(converter = LocalDateAttributeConverter.class)
private LocalDate date;

</pre>

on fera pareil pour les LocalDateTime .