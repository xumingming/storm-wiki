Want to be added to this page? Send an email [here](mailto:nathan.marz@gmail.com).

<table>

<tr>
<td>
<a href="http://groupon.com">Groupon</a>
</td>
<td>
<p>
At Groupon we use Storm to build real-time data integration systems. Storm helps us analyze, clean, normalize, and resolve large amounts of non-unique data points with low latency and high throughput.
</p>
</td>
</tr>

<tr>
<td>
<a href="http://www.rubiconproject.com/">Rubicon Project</a>
</td>
<td>
<p>
Storm is being used in production mode at the Rubicon Project to analyze the results of auctions of ad impressions on its RTB exchange as they occur.  It is currently processing around 650 million auction results in three data centers daily (with 3 separate Storm clusters). One simple application is identifying new creatives (ads) in real time for ad quality purposes.  A more sophisticated application is an "Inventory Valuation Service" that uses DRPC to return appraisals of new impressions before the auction takes place.  The appraisals are used for various optimization problems, such as deciding whether to auction an impression or skip it when close to maximum capacity.
</p>
</td>
</tr>

<tr>
<td>
<a href="http://premise.is/">premise.is</a>
</td>
<td>
<p>
We're building a platform for alternative, bottom-up, high-granularity econometric data capture, particularly targeting opaque developing economies (i.e., Argentina might lie about their inflation statistics, but their black market certainly doesn't). Basically we get to funnel hedge fund money into improving global economic transparency. 
</p>
<p>
We've been using Storm in production since January 2012 as a streaming, time-indexed web crawl + extraction + machine learning-based semantic markup flow (about 60 physical nodes comparable to m1.large; generating a modest 25GB/hr incremental). We wanted to have an end-to-end push-based system where new inputs get percolated through the topology in realtime and appear on the website, with no batch jobs required in between steps. Storm has been really integral to realizing this goal.
</p>
</td>
</tr>

<tr>
<td>
<a href="http://www.ooyala.com/">Ooyala</a>
</td>
<td>
<p>
Ooyala powers personalized multi-screen video experiences for some of the world's largest networks, brands and media companies. We provide all the technology and tools our customers need to manage, distribute and monetize digital video content at a global scale.
</p>

<p>
At the core of our technology is an analytics engine that processes over two billion analytics events each day, derived from nearly 200 million viewers worldwide who watch video on an Ooyala-powered player.
</p>

<p>
Ooyala will be deploying Storm in production to give our customers real-time streaming analytics on consumer viewing behavior and digital content trends. Storm enables us to rapidly mine one of the world's largest online video data sets to deliver up-to-the-minute business intelligence ranging from real-time viewing patterns to personalized content recommendations to dynamic programming guides and dozens of other insights for maximizing revenue with online video.
</p>
</td>
</tr>

<tr>
<td>
<a href="http://rocketfuel.com/">RocketFuel</a>
</td>
<td>
<p>
At Rocket Fuel (an ad network) we are building a real time platform on top of Storm which imitates the time critical workflows of existing Hadoop based ETL pipeline. This platform tracks impressions, clicks, conversions, bid requests etc. in real time. We are using Kafka as message queue. To start with we are pushing per minute aggregations directly to MySQL, but we plan to go finer than one minute and may bring HBase in to the picture to handle increased write load. 
</p>
</td>
</tr>

<tr>
<td>
<a href="http://quicklizard.com/">QuickLizard</a>
</td>
<td>
<p>
QuickLizard builds solution for automated pricing for companies that have many products in their lists. Prices are influenced by multiple factors internal and external to company.
</p>

<p>
Currently we use Storm to choose products that need to be priced. We get real time stream of events from client site and filters them to get much more light stream of products that need to be processed by our procedures to get price recommendation.
</p>

<p>
In plans: use Storm also for real time data mining model calculation that should match products described on competitor sites to client products.
</p>
</td>
</tr>

<tr>
<td>
<a href="http://spider.io/">spider.io</a>
</td>
<td>
<p>
At spider.io we've been using Storm as a core part of our classification engine since October 2011. We run Storm topologies to combine, analyse and classify real-time streams of internet traffic, to identify suspicious or undesirable website activity. Over the past 7 months we've expanded our use of Storm, so it now manages most of our real-time processing. Our classifications are displayed in a custom analytics dashboard, where Storm's distributed remote procedure call interface is used to gather data from our database and metadata services. DRPC allows us to increase the responsiveness of our user interface by distributing processing across a cluster of Amazon EC2 instances.
</p>
</td>
</tr>

<tr>
<td>
<a href="http://8digits.com/">8digits</a>
</td>
<td>
<p>
At 8digits, we are using Storm in our analytics engine, which is one of the most crucial parts of our infrastructure. We are utilizing several cloud servers with multiple cores each for the purpose of running a real-time system making several complex calculations. Storm is a proven, solid and a powerful framework for most of the big-data problems.
</p>
</td>
</tr>

<tr>
<td>
<a href="http://www.fullcontact.com/">FullContact</a>
</td>
<td>
<p>
At FullContact we currently use Storm as the backbone of the system which synchronizes our Cloud Address Book with third party services such as Google Contacts and Salesforce. We also use it to provide real-time support for our contact graph analysis and federated contact search systems.
</p>
</td>
</tr>

<tr>
<td>
<a href="http://twitter.com">Twitter</a>
</td>
<td>
<p>
Storm powers Twitter's publisher analytics product, processing every tweet and click that happens on Twitter to provide analytics for Twitter's publisher partners. Storm integrates with the rest of Twitter's infrastructure, including Cassandra, the Kestrel infrastructure, and Mesos. Many other projects are underway using Storm, including projects in the areas of revenue optimization, anti-spam, and content discovery.
</p>
</td>
</tr>

<tr>
<td>
<a href="https://www.alipay.com/">Alipay</a>
</td>
<td>
<p>
Alipay is China's leading third-party online payment platform. We are using Storm in many scenarios:
</p>

<ol>
<li>
Calculate realtime trade quantity, trade amount, the TOP N seller trading information, user register count. More than 100 million messages per day.
</li>
<li>
Log processing, more than 6T data per day.
</li>
</ol>
</td>
</tr>

<tr>
<td>
<a href="http://navisite.com/">NaviSite</a>
</td>
<td>
<p>
We are using Storm as part of our server event log monitoring/auditing system.  We send log messages from thousands of servers into a RabbitMQ cluster and then use Storm to check each message against a set of regular expressions.  If there is a match (&lt; 1% of messages), then the message is sent to a bolt that stores data in a Mongo database.  Right now we are handling a load of somewhere around 5-10k messages per second, however we tested our existing RabbitMQ + Storm clusters up to about 50k per second.  We have plans to do real time intrusion detection as an enhancement to the current log message reporting system. 
</p>

<p>
We have Storm deployed on the NaviSite Cloud platform.  We have a ZK cluster of 3 small VMs, 1 Nimbus VM and 16 dual core/4GB VMs as supervisors.
</p>
</td>
</tr>

<tr>
<td>
<a href="http://heartbyte.com/">Heartbyte</a>
</td>
<td>
<p>
At Heartbyte, Storm is a central piece of our realtime audience participation platform.  We are often required to process a 'vote' per second from hundreds of thousands of mobile devices simultaneously and process / aggregate all of the data within a second.  Further, we are finding that Storm is a great alternative to other ingest tools for Hadoop/HBase, which we use for batch processing after our events conclude.
</p>
</td>
</tr>


<tr>
<td>
<a href="http://www.alibaba.com/">Alibaba</a>
</td>
<td>
<p>
Alibaba is the leading B2B e-commerce website in the world. We use storm to process the application log and the data change in database to supply realtime stats for data apps.
</p>
</td>
</tr>

<tr>
<td>
<a href="http://www.nodeable.com/">Nodeable</a>
</td>
<td>
<p>
Nodeable uses Storm to deliver real-time continuous computation of the data we consume. Storm has made it significantly easier for us to scale our service more efficiently while ensuring the data we deliver is timely and accurate.
</p>
</td>
</tr>

<tr>
<td>
<a href="https://twitsprout.com/">TwitSprout</a>
</td>
<td>
<p>
At TwitSprout, we use Storm to analyze activity on Twitter to monitor mentions of keywords (mostly client product and brand names) and trigger alerts when activity around a certain keyword spikes above normal levels. We also use Storm to back the data behind the live-infographics we produce for events sponsored by our clients. The infographics are usually in the form of a live dashboard that helps measure the audience buzz across social media as it relates to the event in realtime.
</p>
</td>
</tr>

<tr>
<td>
<a href="http://www.happyelements.com/">HappyElements</a>
</td>
<td>
<p>
<a href="http://www.happyelements.com">HappyElements</a> is a leading social game developer on Facebook and other SNS platforms. We developed a real time data analysis program based on storm to analyze user activity in real time.  Storm is very easy to use, stable, scalable and maintainable.
</p>
</td>
</tr>

<tr>
<td>
<a href="http://www.idexx.com/view/xhtml/en_us/corporate/home.jsf">IDEXX Laboratories</a>
</td>
<td>
<p>
IDEXX Laboratories is the leading maker of software and diagnostic instruments for the veterinary market. We collect and analyze veterinary medical data from thousands of veterinary clinics across the US. We recently embarked on a project to upgrade our aging data processing infrastructure that was unable to keep up with the rapid increase in the volume, velocity and variety of data that we were processing.
</p>

<p>
We are utilizing the Storm system to take in the data that is extracted from the medical records in a number of different schemas, transform it into a standard schema that we created and store it in an Oracle RDBMS database. It is basically a souped up distributed ETL system. Storm takes on the plumbing necessary for a distributed system and is very easy to write code for. The ability to create small pieces of functionality and connect them together gives us the ultimate flexibility to parallelize each of the pieces differently.
</p>

<p>
Our current cluster consists of four supervisor machines running 110 tasks inside 32 worker processes. We run two different topologies which receive messages and communicate with each other via RabbitMQ. The whole thing is deployed on Amazon Web Services and utilizes S3 for some intermediate storage, Redis as a key/value store and Oracle RDS for RDBMS storage. The bolts are all written in Java using the Spring framework with Hibernate as an ORM.
</p>
</td>
</tr>

<tr>
<td>
<a href="http://www.admaster.com.cn/">Admaster</a>
</td>
<td>
<p>
We provide monitoring and precise delivery for Internet advertising. We use Storm to do the following:
</p>

<ol>
<li>Calculate PV, UV of every advertisement.</li>
<li>Simple data cleaning: filter out data which format error, filter out cheating data (the pv more than certain value)</li>
</ol>
Our cluster has 8 nodes, process several billions messages per day, about 200GB.
</td>
</tr>

<tr>
<td>
<a href="http://socialmetrix.com/en/">SocialMetrix</a>
</td>
<td>
<p>
Since its release, Storm was a perfect fit to our needs of real time monitoring. Its powerful API, easy administration and deploy, enabled us to rapidly build solutions to monitor presidential elections, several major events and currently it is the processing core of our new product "Socialmetrix Eventia".
</p>
</td>
</tr>

<tr>
<td>
<a href="http://www.taobao.com/index_global.php">Taobao</a>
</td>
<td>
<p>
We make statistics of logs and extract useful information from the statistics in almost real-time with Storm.  Logs are read from Kafka-like persistent message queues into spouts, then processed and emitted over the topologies to compute desired results, which are then stored into distributed databases to be used elsewhere. Input log count varies from 2 millions to 1.5 billion every day, whose size is up to 2 terabytes among the projects.  The main challenge here is not only real-time processing of big data set; storing and persisting result is also a challenge and needs careful design and implementation.
</p>
</td>
</tr>

<tr>
<td>
<a href="http://needium.com/">Needium</a>
</td>
<td>
<p>
At Needium we love Ruby and JRuby. The Storm platform offers the right balance between simplicity, flexibility and scalability. We created RedStorm, a Ruby DSL for Storm, to keep on using Ruby on top of the power of Storm by leveraging Storm's JVM foundation with JRuby. We currently use Storm as our Twitter realtime data processing pipeline. We have Storm topologies for content filtering, geolocalisation and classification. Storm allows us to architecture our pipeline for the Twitter full firehose scale.
</p>
</td>
</tr>

<tr>
<td>
<a href="http://parse.ly/">Parse.ly</a>
</td>
<td>
<p>
Parse.ly is using Storm for its web/content analytics system. We have a home-grown data processing and storage system built with Python and Celery, with backend stores in Redis and MongoDB. We are now using Storm for real-time unique visitor counting and are exploring options for using it for some of our richer data sources such as social share data and semantic content metadata.
</p>
</td>
</tr>

<tr>
<td>
<a href="http://www.crowdflower.com/">CrowdFlower</a>
</td>
<td>
<p>
CrowdFlower is using Storm with Kafka to generalize our data stream
aggregation and realtime computation infrastructure. We replaced our
homegrown aggregation solutions with Storm because it simplified the
creation of fault tolerant systems. We were already using Zookeeper
and Kafka, so Storm allowed us to build more generic abstractions for
our analytics using tools that we had already deployed and
battle-tested in production.
</p>

<p>
We are currently writing to DynamoDB from Storm, so we are able to
scale our capacity quickly by bringing up additional supervisors and
tweaking the throughput on our Dynamo tables. We look forward to
exploring other uses for Storm in our system, especially with the
recent release of Trident.
</p>
</td>
</tr>

<tr>
<td>
<a href="http://www.dsbox.com">Digital Sandbox</a>
</td>
<td>
<p>
At Digital Sandbox we use Storm to enable our open source information feed monitoring system.  The system uses Storm to constantly monitor and pull data from structured and unstructured information sources across the internet.  For each found item, our topology applies natural language processing based concept analysis, temporal analysis, geospatial analytics and a prioritization algorithm to enable users to monitor large special events, public safety operations, and topics of interest to a multitude of individual users and teams.
</p>
 
<p>
Our system is built using Storm for feed retrieval and annotation, Python with Flask and jQuery for business logic and web interfaces, and MongoDB for data persistence. We use NTLK for natural language processing and the WordNet, GeoNames, and OpenStreetMap databases to enable feed item concept extraction and geolocation.
</p>
</td>
</tr>

<tr>
<td>
<a href="http://www.mercadolibre.com/">MercadoLibre</a>
</td>
<td>
</td>
</tr>

</table>