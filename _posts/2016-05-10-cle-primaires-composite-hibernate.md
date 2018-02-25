---
id: 142
title: Clé primaires composite en Hibernate
date: 2016-05-10T21:23:46+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=142
permalink: /cle-primaires-composite-hibernate/
categories:
  - JAVA
---
Pour créer une clé composée on utilise **EmbeddedId** ou **IdClass** .

Une clé composite doit respecter les règles suivantes : 

  * la classe qui représente la clé composite doit être publique
  * la classe doit déclarer un constructeur public vide son argument
  * la classe doit etre sérailisable 
  * La classe doit redéfinir les méthodes **equals** et **hashCode**
La clé composite doit être annoté par @Embeddable commee suit :
  
`<br />
@Embeddable<br />
public class compositePK implements Serializable {</p>
<p>    private Integer idOne;<br />
    private Integer idTwo;</p>
<p>    public TimePK() {}</p>
<p>    public compositePK (Integer idOne, Integer idTwo) {<br />
        this.idOne= idOne;<br />
        this.idTwo= idTwo;<br />
    }<br />
    // equals, hashCode<br />
}<br />
` 

la classe doit redifinir les deux méthode de facon propre sinon elle ne marchera pas .car Hibernate doit etre capable de comparer les composite ID . généralement on doit utiliser tous les champs dans la redifinition des methodes Hashcode et equals.

On spécifie une clé composite dans la l&rsquo;entité en utilisant l&rsquo;annotation @EmbeddedId
  
`<br />
@Entity(name="")<br />
public class Entity implements Serializable {</p>
<p>    @EmbeddedId<br />
    private CompositePK id;</p>
<p>   // getter and setter </p>
<p>}<br />
`