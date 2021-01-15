---

copyright:
  years: 2015, 2020
lastupdated: "2020-02-12"

keywords: IBM Event Streams, Kafka as a service, managed Apache Kafka

subcollection: EventStreams

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .note}
{:important: .important}

# Getting started tutorial
{: #getting_started}

{{site.data.keyword.messagehub_full}} is a high-throughput message bus built with Apache Kafka. To get started with {{site.data.keyword.messagehub}}
and start sending and receiving messages, you can use the Java™ sample. The sample shows how a producer sends
messages to a consumer using a topic. The same sample program is used to consume messages and
produce messages.
{: shortdesc}

To understand more about how {{site.data.keyword.messagehub}} works, see [About {{site.data.keyword.messagehub}}](/docs/EventStreams?topic=EventStreams-about). {{site.data.keyword.messagehub}} was previously called Message Hub.

To access other {{site.data.keyword.messagehub}} samples, including samples for Node.js and Python, see [{{site.data.keyword.messagehub}} samples ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-messaging/event-streams-samples){:new_window}.

<!-- 11/01/18 - Karen - removing diagram as requested by James
![Java sample overview diagram](getting_started_sample.gif "Overview diagram of Java sample showing the flow of messages.")
-->
<!-- 08/08/2019 - Chloe - Re-structuring to get UI components of the flow introduced earlier in the flow. Also moving pre-requsisites to a potentially collapsible section. -->

## Prerequisites
{: #getting_started_prereqs}

1. **If you don't already have one, create an {{site.data.keyword.messagehub}} service instance.**

   a. Log in to the {{site.data.keyword.Bluemix_notm}} console.
  
   b. Click the [**{{site.data.keyword.messagehub}} service** ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/catalog/event-streams){:new_window} in the **Catalog**.
  
   c. Select the **Lite plan** on the service instance page.
  
   d. Enter a name for your service. You can use the default value.
  
   e. Click **Create**. The {{site.data.keyword.messagehub}} **Getting started** page opens. 

2. **If you don't already have them, install the following prerequisites:**
	
	* [git ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://git-scm.com/){:new_window}
	* [Gradle ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://gradle.org/){:new_window}
	* Java 8 or higher

## Tutorial steps
{: #getting_started_steps}

1. {: #Create_topic_step notoc} **Create a topic **

   The topic is the core of {{site.data.keyword.messagehub}} flows. Data passes through a topic from producing applications to consuming applications. 

   We'll be using the {{site.data.keyword.Bluemix_notm}} console (UI) to create the topic, and will reference it when starting the application.

      a. Go to the **Topics** tab.
  
      b. Click **New topic**.
  
      c. Name your topic.
  
     The sample application is configured to connect to topic `kafka-java-console-sample-topic`. If the topic does not exist, it is created when the application is started. 
     {: important}

      d. Keep the defaults set in the rest of the topic creation, click **Next** and then **Create topic**.

      e. The topic appears in the table. You have created a topic!
  
2. {: #create_credentials_step notoc} **Create credentials**

    To allow the sample application to access your topic, we need to create some credentials for it. 

     a. Go to **Service credentials** in the navigation pane.
  
     b. Click **New credential**.
  
     c. Give the credential a name so you can identify its purpose later. You can accept the default value.
  
     d. Give the credential the **Manager** role so that it can access the topics, and create them if necessary. 
  
     e. Click **Add**. The new credential is listed in the table in **Service credentials**.
  
     f. Click **View credentials** to see the `api_key` and `kafka_brokers_sasl` values.

3. {: #clone_repository_step notoc} **Clone the Github repository for the sample application**

   The sample application is stored in Github. Clone the `event-streams-samples` repository by running the clone command from the command line. 

   ```
   git clone https://github.com/ibm-messaging/event-streams-samples.git
   ```
   {: codeblock}

   <br/>
   When the repository is cloned, from the command line change into the <code>kafka-java-console-sample</code> directory.

   ```
   cd event-streams-samples/kafka-java-console-sample
   ```
   {: codeblock}

   <br/>
   Build the contents of the <code>kafka-java-console-sample</code> directory.

   ```
   gradle clean && gradle build
   ```
   {: codeblock}

4. {: #start_consumer_step notoc} **Run the consuming application**
   
   Start the sample consuming application from the command line, replacing the `kafka_brokers_sasl` and `api_key` values. 

   The `java -jar ./build/libs/kafka-java-console-sample-2.0.jar` part of the command identifies the locations of the .JAR file to run within the cloned repository. You do not need to change this. 
   
   Use the `kafka_brokers_sasl` from the **Service credentials** created in [Step 2](/docs/EventStreams?topic=EventStreams-getting_started#create_credentials_step). We recommend using all the `kafka_brokers_sasl` listed in the **Service credentials** that you created.

   The `kafka_brokers_sasl` must be formatted as `"host:port,host2:port2"`. </br> Format the contents of `kafka_brokers_sasl` in a text editor before entering it in the command line.
   {: important}

   Then, use the `api_key` from the **Service credentials** created in [Step 2](/docs/EventStreams?topic=EventStreams-getting_started#create_credentials_step). `-consumer` specifies that the consumer should start. 

   ```
   java -jar ./build/libs/kafka-java-console-sample-2.0.jar 
   <kafka_brokers_sasl> <api_key> -consumer
   ```
   {: codeblock}

   An `INFO No messages consumed` is displayed when the consuming application is running, but there is no data being consumed. 

5. {: #start_producer_step notoc} **Run the producing application**

   Open a new command line window and change into the <code>kafka-java-console-sample</code> directory.

   ```
   cd event-streams-samples/kafka-java-console-sample
   ```
   {: codeblock}
   
   Then, start the sample producing application from the command line, replacing the `kafka_brokers_sasl` and `api_key` values. 

   The `java -jar ./build/libs/kafka-java-console-sample-2.0.jar` part of the command identifies the locations of the .JAR file to run within the cloned repository. You do not need to change this. 

   Use the `kafka_brokers_sasl` from the **Service credentials** created in [Step 2](/docs/EventStreams?topic=EventStreams-getting_started#create_credentials_step). We recommend using all the `kafka_brokers_sasl` listed in the **Service credentials** that you created.

   The `kafka_brokers_sasl` must be formatted as `"host:port,host2:port2"`. </br> Format the contents of `kafka_brokers_sasl` in a text editor before entering it in the command line.
   {: important}

   Use the `api_key` from the **Service credentials** created in [Step 2](/docs/EventStreams?topic=EventStreams-getting_started#create_credentials_step). `-producer` specifies that the producer should start. 

   ```
   java -jar ./build/libs/kafka-java-console-sample-2.0.jar
	<kafka_brokers_sasl> <api_key> -producer
   ```
   {: codeblock}

6. {: #success_step notoc} **Success!**

   When the producer starts, messages are produced to the topic. Messages are then consumed from the topic by the consuming application.
   You can verify the successful flow of messages when you see`INFO Message consumed` from the consumer. 

   The sample runs indefinitely until you stop it. To stop the process, run an exit command `Ctrl+C`.

## Next steps
{: #next_steps}

Now that you've run the Java sample application, you can try other [{{site.data.keyword.messagehub}} samples ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/ibm-messaging/event-streams-samples){:new_window}, explore [other ways to connect ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/EventStreams?topic=EventStreams-kafka_connect){:new_window} to the {{site.data.keyword.messagehub}} service, take a look at the [IBM Event Streams on IBM Cloud Private and Red Hat OpenShift ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/garage/dte/tutorial/ibm-event-streams-tutorial-part-1) tutorial or find out more about 
[{{site.data.keyword.messagehub}} on IBM Cloud Private ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://ibm.github.io/event-streams/){:new_window}.

To watch a video that walks
you through getting this Java sample to run, see [Getting started with IBM {{site.data.keyword.messagehub}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.youtube.com/watch?v=XyNy7TcfJOc){:new_window}.
 
 
<!-- 07/06/18 - Karen: removing until a newer version available
To watch a video that walks
you through getting a Java sample to run against {{site.data.keyword.messagehub}}, see [{{site.data.keyword.messagehub}} - Getting started with IBM's Kafka in the cloud ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.youtube.com/watch?v=tt-bLtFzC_4){:new_window}.
-->





