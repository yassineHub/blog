---
id: 164
title: parcourir les arboresences en java et en jsp
date: 2016-06-10T16:13:20+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=164
permalink: /afficher-arborescence-jsp/
categories:
  - JAVA
---
la modélisation d&rsquo;une structure d&rsquo;arboresence au niveau java se fait comme suit : 

`</p>
<p>public class Node{</p>
<p>   private String name ;<br />
   private List<Node> childreen;</p>
<p>   ...<br />
   //getters and setters<br />
}<br />
` 

Le principe pour pourvoir afficher une arboresence en jsp est de créer une jsp qui appelle elle même en boucle 

Dans la jsp principale :

`</p>
<ul>
<c:set var="node" value="${node}" scope="request" /><br />
<jsp:include page="tree.jsp"/>
</ul>
<p>`

et dans la page tree.jsp

`</p>
<li >
<span>${node.name}</span><br />
<c:if test="${fn:length(node.childreen) > 0}"></p>
<ul>
<c:forEach var="node" items="${node.childreen}"><br />
    <c:set var="node" value="${node}" scope="request"/><br />
    <jsp:include page="tree.jsp"/><br />
</c:forEach>
</ul>
<p></c:if>
</li>
<p>`

&#8212; Pour parcourir une arbboresence de facon récursive , il faut faire comme suit :
  
`<br />
public static void traverse(Node node)<br />
{<br />
   // traitement sur node courant </p>
<p>   List<Node> childreen = node.getChildreen();<br />
   for (Node node : childreen)<br />
   {<br />
         // traitement sur chaque enfant du noeud courant<br />
         traverse(node);</p>
<p>   }<br />
}<br />
`