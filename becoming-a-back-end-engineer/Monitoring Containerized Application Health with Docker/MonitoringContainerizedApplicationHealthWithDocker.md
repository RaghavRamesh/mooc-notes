### Module 1 - Course overview

Monitoring is one of the biggest advantages Docker brings. Number 1 barrier from migrating applications to containers has been monitoring

Course objectives
- How monitoring works and how to visualise in dashboard
- How to build 3 levels of monitoring in dashboard
- How to make monitoring consistent across different tech stacks

Module overview
1. Monitoring Architecture
2. Collecting Metrics using Prometheus
3. Runtime Metrics - metrics collected by OS and web server
4. Application Metrics - custom stats relevant to application
5. Docker / Container Metrics 
6. Building dashboards using Grafana

* * *

### Module 2 - Architecting monitoring for containerised applications

- Monitoring apps in containers is different from monitoring apps on servers.
- You may be used to centralised monitoring using Nagios in Linux world and SCOM in the Windows world. There you have agents listening to metrics on application servers and pushing them to central servers.
- This approach doesn’t make sense for containers because containerised apps are more dynamic. Containers stop and start in seconds which helps instant scaling up and down. Application servers are long lived.
- All containerised expose a /metrics REST API endpoint which returns a snapshot of all the metrics that are captured
- Main role of Prometheus is to collect and store metrics from all other components. The Prometheus container calls the REST API of all the other containers and of the Docker servers on a schedule. Then it stores all the metrics in a time series database which you can query to get data at a single point in time or aggregated over a long period of time.
- Grafana is used for visualising the metrics in dashboards.

Prometheus - covered in detail in a later module
- Configuring metric sources
    - Configured with a yaml file
    - It is a polling server. It reads metrics by making GET requests to target URLs on a timed schedule.
    - All the polling happens in the background.
    - It has its own database to store the metrics and also has a basic UI to check on the setup and querying the database.
- Using the prometheus UI
- Querying metrics with promQL

Consistent monitoring with containers
- We should build the /metrics API endpoint so that Prom has something to read - simple architecture but a clever way to get monitoring information across the whole application
- Every component has its own metrics endpoint.
- Collecting the metrics should be a cheap operation and we control how often Prom polls the data from the metrics endpoint

Docker platform metrics
Great thing about monitoring containerised apps is the consistency
- Container status
- Server metrics
- Swarm node status

Module summary - Containerised app monitoring
- Consistency - every part of the app gets monitored the same way with the same technologies and we can run the exact same monitoring tools in every environment
- Core of it is adding a metrics endpoints in containers and exposing metrics
- Enabling metrics from the Docker platform
- Then run Prometheus metrics server in one container to collect the stats
- Then run Grafana visualisation in another container to visualise the stats

* * *

Module 3 - Collecting Metrics with Prometheus

A metrics server is the central point for collecting and storing monitoring data in containerised applications. Prometheus is the most popular metrics server. It’s open-source, cross-platform, Docker-friendly and extremely powerful. 

Module overview
- Running Prometheus in Docker
    - Official Linux image
    - Custom Windows image
- Configuring Prometheus
    - Specifying metric targets
    - Applying runtime configuration
- Prometheus Data Types
    - Counters and gauges
    - Querying data

Running Prometheus in Docker
- Why should it run in its own container?
    - Prometheus is a server application written in Go which is light-weight and cross-platform
    - You can run Prometheus directly on a server as part of your cluster or in a separate server on your network but it’s more beneficial to run it as a container along side your application containers. That’s partly for the same reason it’s better to run any application in a container; the portability, efficiency and security from the Docker platform.
    - But for Prometheus it’s more beneficial because it can then be an internal component that isn’t publicly accessible. Containers in the Docker network can access each other without their ports published to the outside world.
    - None of the prometheus endpoints or the metrics endpoints are publicly accessible so they won’t be compromised
    - Being in a container itself lets Prometheus discover all the target components because Docker platform provides service discovery
    - Prometheus is a stateful application though so need to be mindful when running multiple instances for high availability

Configuration and service discovery in Prometheus
You’ll run your Prometheus container in the same Docker network as the application container.
You’ll configure P to poll those containers as scrape targets and
set a schedule to how often P makes an HTTP GET request to the /metrics endpoints in the containers

- Job configs
    - The target configuration is the server address or IP address and port.
    - Add a job for each application you want to monitor and specify the container name as the target

##### Understanding Prom Data Types
* Counter
    * Simple number
    * Always increases
    * Use for incrementing metrics
        * Requests processed
        * Errors handled
* Gauge
    * Simple number
    * Can inc or dec
    * Use fo measurements and snapshots
        * Memory consumption 
        * Current thread count
* Histograms & Summaries
    * Groups of samples
    * Record count of samples and total
    * Use for quantiles
        * 95th percentile SLA
    * Not supported by all clients

***

### Module 4 - Exposing Runtime Metrics to Prometheus

* All the major application runtimes track their own metrics. Java has JVM Metrics, .NET has Windows perf counters.
* Web servers like Tomcat and IIS also collect metrics - such as useful data about requests and responses. 
* A lot of useful information is already collected by the runtime, we just need to expose it from the container

Exporter
* Common pattern 
* A generic utility application that exposes metrics for a particular runtime that Prom can poll
* For apps that are not actively developed, an exporter utility can be created to expose the already made available runtime metrics
* Many exporter tools available some of which are supported by the Prometheus team

