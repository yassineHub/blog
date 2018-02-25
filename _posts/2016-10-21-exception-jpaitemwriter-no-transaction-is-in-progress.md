---
id: 274
title: 'Exception JpaItemWriter: no transaction is in progress'
date: 2016-10-21T15:36:36+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=274
permalink: /exception-jpaitemwriter-no-transaction-is-in-progress/
categories:
  - JAVA
---
<span style="text-decoration: underline;"><strong>Problème :</strong></span>

Cette erreur Hibernate arrive généralement quand dans une step du spring batch ( ) on utilise la même transaction que celle du JobRepository .

Comme illustré dans le code ci dessous , on ne précise pas de transaction à la step  , du coup il va utiliser la transaction par défaut configuré pour le batch dans le jobRepository .

<pre class="brush: java; title: ; notranslate" title="">@Bean
public Step readProcessWriteItemsStep(){

return stepBuilderFactory.get("readProcessWriteItemsStep")

.&lt;OrderCesame,OrderCesame&gt;chunk(100)
.reader(dataBaseReader())
.processor(itemProcessor)
.writer(databaseWriter())
.build()
;
}

</pre>

&nbsp;

L&rsquo;erreur qu&rsquo;on a  :

<pre class="brush: plain; title: ; notranslate" title="">TransactionRequiredException: no transaction is in progress
</pre>

**Solution :**

En fait  la step « read/process/write » ne doit pas utiliser le même transaction manager du job . pour cela il suffit de créer un nouveau transaction manager qu&rsquo;on doit injecter dans la step :

<pre class="brush: java; title: ; notranslate" title="">@PersistenceContext(unitName="PU")
@Qualifier(value ="entityManagerFactory")
private EntityManager em;

&nbsp;
 @Bean
 @Qualifier("jpaTrx")
 public PlatformTransactionManager jpaTransactionManager() {
 return new JpaTransactionManager(em.getEntityManagerFactory());
 }


@Bean
public Step readProcessWriteItemsStep(){

return stepBuilderFactory.get("readProcessWriteItemsStep")
.transactionManager(jpaTransactionManager())
.&lt;OrderCesame,OrderCesame&gt;chunk(100)
.reader(dataBaseReader())
.processor(itemProcessor)
.writer(databaseWriter())
.build()
;
}

</pre>