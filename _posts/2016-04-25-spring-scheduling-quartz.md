---
id: 74
title: 'Spring scheduling  avec Quartz'
date: 2016-04-25T22:31:12+00:00
author: admin
layout: post
guid: http://boussoufiane.com/?p=74
permalink: /spring-scheduling-quartz/
categories:
  - JAVA
---
  * **<span style="text-decoration: underline;">Introduction </span>**

<div>
  Depuis la JDK 1.3 , java propose un système de planification de tache avec la classe Timer . ce système  offre des fonctionnalités  très basique et très simples .
</div>

<div>
</div>

<div>
  Quartz est un framwork  parmi tant d&rsquo;autres qui ont vu le jour  et qui est très interessant puisqu&rsquo;il  permet de gérer des processus assez complexe de planification de tache et  dispose de plusieurs fonctionnalité qui conviennnet à la plupart des applications métiers .
</div>

<div>
  parmi les principaux points fort de quartz  :
</div>

<div>
  &#8211; Flexibelité d&rsquo;exécution dans des environement varié ( integration dans une application standalone , instanciation dans un encironement j2EE distribué , environement distant avec appel RMI )
</div>

<div>
  -facilité d&rsquo;integration avec d&rsquo;autres framwork tel que  java et sping
</div>

<div>
  &#8211; la persistence en base de donnée ou en memoire des des élements de planification (jobs , triggers)
</div>

<div>
  &#8211; Gestion des transaction
</div>

<div>
  <p>
    &#8211; Clustering
  </p>
  
  <ul>
    <li>
      Fail-over.
    </li>
    <li>
      Load balancing.
    </li>
  </ul>
  
  <div>
    Spring dans sa 3eme version propos des interfaces (TaskScheduler) pour permettre l&rsquo;integration de quartz et du scheduler Timer java
  </div>
  
  <div>
    Dans cet article on va se concentrer sur l&rsquo;integration de Quartz avec Spring
  </div>
  
  <div>
  </div>
  
  <div>
    Quartz utilise 4 composant pour gérer la planification des taches dans Quartz  :
  </div>
  
  <div>
    &#8211; Job : Tache à exécuter
  </div>
  
  <div>
    &#8211; JobDetail : object contenant les information du job et son éxécution
  </div>
  
  <div>
    &#8211; Trigger : Object qui gère l&rsquo;ordonnancement
  </div>
  
  <div>
    &#8211; scheduler : Permet de configurer Quartz
  </div>
</div>

### <span style="text-decoration: underline;">Job Quartz </span>

on peut créer un job en implémentant l&rsquo;interface Job de quartz

<pre class="brush: java; title: ; notranslate" title="">public class TaskJob&amp;amp;amp;amp;amp;amp;amp;nbsp;implements Job {

public void execute(JobExecutionContext context) throws JobExecutionException {
System.out.println("executing task job");
}
}

</pre>

JobExecutionContext contient toutes les informations concernant le&rsquo;exécution du job à savoir les information du job lui meme ainsi que le trigger

### <u>**Quartz JobBuilder**</u>

Dans le JobDetail on spécifie notre class Job et les paramètres qu&rsquo;on veut passer  à notre job

<pre class="brush: java; title: ; notranslate" title="">@Bean
public JobDetail jobDetail() {
 return JobBuilder.newJob().(TaskJob.class)
 .storeDurably()
 .withIdentity("Qrtz_Job_Detail")
 .withDescription("Invoke Sample Job service...")
 .build();
}
jobDetail.getJobDataMap().put("param1", 1);
jobDetail .getJobDataMap().put( "param2" , "toto");
jobDetail .getJobDataMap().put( "param3" , new Toto());

</pre>

**Spring _JobDetailFactoryBean_**

Spring propose la classe _** **_**_JobDetailFactoryBean pour  créer le jobDetail , et c&rsquo;est pareil que la configuration du job builder de quartz vu  ci dessus _**

<pre class="brush: java; title: ; notranslate" title="">@Bean
public JobDetailFactoryBean jobDetail() {
    JobDetailFactoryBean jobDetailFactory = new JobDetailFactoryBean();
    jobDetailFactory.setJobClass(TaskJob.class);
    jobDetailFactory.setDescription("Invoke Sample Job service...");
    jobDetailFactory.setDurability(true);
    return jobDetailFactory;
}
</pre>

**Quartz _TriggerBuilder_**

Le trigger c&rsquo;est lui qui gère l&rsquo;ordonnancement  des taches  , c&rsquo;est à dire les moment auquel on veut lancer nos taches .

<pre class="brush: java; title: ; notranslate" title="">@Bean
public Trigger trigger(JobDetail job) {
    return TriggerBuilder.newTrigger().forJob(job)
      .withIdentity("Quartz_Trigger")
      .withDescription("trigger")
      .withSchedule(CronScheduleBuilder.cronSchedule("0/10 * * * * ?"))

      .build();
}
</pre>

Ici la tche sera exécuté chaque 10 secondes
  
**Spring _SimpleTriggerFactoryBean_**
  
Spring propose la classe _** **_ **_SimpleTriggerFactoryBean_**  **_pour  créer des trigger_**

<pre class="brush: java; title: ; notranslate" title="">@Bean
public SimpleTriggerFactoryBean trigger(JobDetail job) {
    SimpleTriggerFactoryBean trigger = new SimpleTriggerFactoryBean();
    trigger.setJobDetail(job);
    trigger.setRepeatInterval(3600000);
    trigger.setRepeatCount(SimpleTrigger.REPEAT_INDEFINITELY);
    return trigger;
}
</pre>

**Spring _SchedulerFactoryBean_**
  
_**cette classe permet de configurer les differents paramètres du scheduler ,
  
à savoir la base de données , la transaction manager , le job ,
  
le triger et les propriétés du scheduler.**_

&nbsp;

<pre class="brush: java; title: ; notranslate" title="">@Bean
public SchedulerFactoryBean scheduler(Trigger trigger, JobDetail job) {
    SchedulerFactoryBean schedulerFactory = new SchedulerFactoryBean();
	
	// set data sources
    schedulerFactory .setDataSource(dataSource);
    schedulerFactoryBean.setTransactionManager(transactionManager);

	// Set scheduler properties
    Properties props = new Properties();
    props.setProperty("org.quartz.jobStore.driverDelegateClass","org.quartz.impl.jdbcjobstore.MSSQLDelegate");
    schedulerFactoryBean.setQuartzProperties(props);

	// we can set properties from an external file
    //schedulerFactory.setConfigLocation(new ClassPathResource("quartz.properties"));


    schedulerFactory.setJobFactory(springBeanJobFactory());
    schedulerFactory.setJobDetails(job);
    schedulerFactory.setTriggers(trigger);
    return schedulerFactory;
}
</pre>

_**on peut externaliser les propriérs du scudeler dans un fichier de properties en faisant : **_

<pre class="brush: java; title: ; notranslate" title="">schedulerFactory.setConfigLocation(new ClassPathResource("quartz.properties"));
</pre>

Spring Job :

<div>
  on peut également créer un job en étendant la classe QuartzJobBean de spring . cette classe  permet de mapper de facon automatique  les propriétés ajouté dans le JobDataMap avec les champs déclarés dans la classe du job , ces champs seront injecté à notre job à chaque fois qu&rsquo;il s&rsquo;exécute :
</div>

<div>
</div>

<div>
  <pre class="brush: java; title: ; notranslate" title="">
	jobDetail.getJobDataMap().put("stringParam", "chaine de caractère");
	jobDetail.getJobDataMap().put("serviceObjectParam", serviceInterface);

</pre>
</div>

<div>
</div>

<div>
</div>

<div>
   les clés de nos paramètres doivent avoir les même nom que les champs déclarés dans notre job et veiller à ce qu&rsquo;on définit des getter et des setter à ces champs !!
</div>

<div>
  <pre class="brush: java; title: ; notranslate" title="">
public class Job2 extends QuartzJobBean {
	
	private String stringParam  ; 
	private IService serviceObjectParam ; 

	@Override
	protected void executeInternal(JobExecutionContext context) 
throws JobExecutionException {
	

		 System.out.println("string Parameter: " + stringParam );
		 serviceObjectParam.serviceMethod();
		
	}
</pre>
  
  <p>
    Les paramètres qu&rsquo;on injecte dans notre job devoient etre serialisable car ils seront<br /> sauvegardés dans la table <em>qrtz_job_details</em>
  </p>
  
  <p>
    Injection des bean spring depuis le contexte application
  </p>
  
  <p>
    l&rsquo;objet <a href="http://static.springsource.org/spring/docs/3.0.x/javadoc-api/org/springframework/scheduling/quartz/SpringBeanJobFactory.html">SpringBeanJobFactory</a> permet d&rsquo;injecter dans le job les champs qui sont dans le<br /> contexte du Scheduler à savoir , les JobDataMap et les données des triggers .
  </p>
  
  <p>
    mais ne permet pas d&rsquo;injecter des beans depuis le contexte d&rsquo;application spring .<br /> car Quartz crée une nouvelle instance à chaque éxécution , du coup le job ne peut pas etre considéré comme un bean spring . du coup on doit implémenter notre propre jobFactory qui surcharge celle qui est configuré par défaut
  </p>
</div>

<div>
  on définit notre nouvelle jobFactory :
</div>

<div>
  <pre class="brush: java; title: ; notranslate" title="">
public final class AutowiringSpringBeanJobFactory
    extends SpringBeanJobFactory
    implements ApplicationContextAware {
 
  private transient AutowireCapableBeanFactory beanFactory;
 
  public void setApplicationContext(
      final ApplicationContext context) {
    beanFactory = context.getAutowireCapableBeanFactory();
  }
 
  @Override
  protected Object createJobInstance(
        final TriggerFiredBundle bundle)
      throws Exception {
    final Object job = super.createJobInstance(bundle);
    beanFactory.autowireBean(job);
    return job;
  }
}
</pre>
  
  <p>
    <span style="font-size: 0.95em; line-height: 1.6em;">]</span>
  </p>
</div>

<div>
</div>

<div>
  qu&rsquo;on va injecter dans l&rsquo;objet SchedulerFactoryBean
</div>

<div>
</div>

<div>
  <pre class="brush: java; title: ; notranslate" title="">
 SchedulerFactoryBean schedulerFactoryBean =  new SchedulerFactoryBean();
 AutowiringSpringBeanJobFactory jobFactory = new AutowiringSpringBeanJobFactory();	
 jobFactory.setApplicationContext(applicationContext);
 schedulerFactoryBean.setJobFactory(jobFactory)
</pre>
</div>

<div>
</div>

<div>
  Quartz table en Base de données
</div>

<div>
</div>

<div>
  Les tables pour sauvegarder les informations des job quartz doivent être crée manuellement par des script qu&rsquo;on trouve dans le zip qu&rsquo;on télécharge sur le site officiel de Quartz .
</div>

<div>
  le zip contient un script par base de données .
</div>

<div>
</div>

<div>
</div>

<div>
</div>