# Load Testing on Cloud API



## Introduction to load test on Cloud API &#123;#intro&#125;

### What is an API load test? &#123;#what&#125;

API Load Testing is a crucial process for assessing how well your application servers handle the expected production traffic. It allows you to systematically apply the anticipated load to your servers and observe how your application behaves. By conducting API Load Testing, you can identify any weaknesses or bottlenecks before your users encounter them, ensuring that your application is robust enough to handle the desired workload.

The WhatsApp Cloud API provides a feature that enables you to simulate traffic on the messaging API endpoint and evaluate its performance under various conditions. This feature lets you examine the overall performance of your API integration and assess its ability to handle concurrent requests from a large number of virtual users.

This feature helps you test whether your messaging services can handle increasing user demands and traffic fluctuations.

### What types of load tests are available? &#123;#types-of-load-tests&#125;

At present, our Cloud API offers &quot;Outbound Load Test.&quot; This feature serves as a powerful tool for businesses to evaluate and enhance the resiliency of their WhatsApp Cloud API integration. By conducting Outbound Load Testing, clients can gain insightful measurements regarding their system&#039;s performance and identify areas for improvement.

The ability to proactively assess and optimize their systems is paramount for businesses aiming to provide seamless messaging experiences for their customers, even during periods of peak usage. By leveraging the Outbound Load Test feature, clients can gain confidence in the robustness and scalability of their WhatsApp Cloud API integration, ensuring that it can handle high message volumes without sacrificing performance or user experience.

### Why should you perform outbound load testing using Cloud API? &#123;#why&#125;

Performing Outbound Load Testing using our Cloud API offers significant benefits in two distinct scenarios:

1. **Solution Partners Tracking**: Solution Partners can leverage load testing to track the time series of API requests made and the corresponding responses received. This enables Solution Partners to gain valuable insights into the performance of their systems over time. By monitoring these metrics, Solution Partners can identify patterns, analyze response times, and ensure optimal service delivery to their clients.

2. **Message Status Webhook Monitoring**: For businesses, load testing provides a means to monitor the number of message status webhooks received on their designated endpoint during the testing period. By analyzing webhook data, businesses can gauge the responsiveness and reliability of their systems in real-time. This allows them to proactively detect any issues, optimize their webhook handling processes, and ensure seamless message status updates for their users.

In both scenarios, load testing serves as a powerful tool for assessing and improving webhook performance. Whether it is tracking API request-response dynamics or monitoring message status webhooks, load testing empowers businesses and Solution Partners to enhance their services, optimize workflows, and deliver exceptional user experiences.

## Preparing for the load test &#123;#preparing&#125;

To ensure a smooth and effective load test experience, appropriate preparation is crucial. In the case of an outbound load test with Meta, Meta provides essential components to facilitate the process. Specifically, Meta provides a designated test number that serves as the sender of the load test messages. Additionally, Meta provides a range of dummy numbers to act as end user numbers for the test.

The test number Meta provides has been purposefully designed to process all the requests similar to a regular number would, but it will not deliver the messages to actual recipients. This thoughtful design prevents any inadvertent spamming of real numbers during the load test, maintaining the integrity of the process.

To initiate the load test, clients are required to create load test requests via our Direct Support channel. This ensures streamlined communication and enables our team to provide prompt assistance and guidance throughout the testing phase.

To set up the load test successfully, provide the following information:

1. **Existing Message Template Example**: Provide a sample of the message template you intend to use for the load test. This includes the associated name and WhatsApp Business account (WABA) linked to it. This information allows Meta to accurately replicate your messaging setup during the load test, ensuring realistic testing conditions.

2. **Webhook URL**: Please provide us with the URL of the webhook endpoint that will receive notifications and data from the load test. This enables us to capture and analyze the responses and behavior of your system during the test, allowing for thorough evaluation and optimization.

3. **Webhook URL verify token**: Provide the webhook verify token so that Meta can override your Webhook URL to receive notifications.

Once the load test session is confirmed, our dedicated support team will provide you with the necessary resources on the day of the load test, including:

- **Test Number**: Meta provides a dedicated test number that serves as the sender for the load test messages. This number is specifically designed to process all the requests like a regular number would, without delivering the messages to actual recipients. This ensures the load test is conducted accurately while safeguarding the privacy of real numbers.

- **Access Token**: You will receive an access token that grants you the necessary authorization to initiate and monitor the load test.

- **Message Template**: Meta provides the specific message template and its corresponding details to be used during the testing. This includes the content, structure, and any variables or placeholders that need to be included within the messages.

Our support team will be available throughout the load test to provide guidance, address any queries, and ensure a smooth testing experience.

**Note**: It is crucial to ensure that no incoming messages are sent to the test number when registering or obtaining the ID of the outbound number. Sending a message to the test number during the load test can trigger a circuit breaker mechanism, resulting in the disconnection of the load test number. To maintain a stable and uninterrupted load test session, please refrain from sending any messages to the designated test number during the setup and execution of the load test.

## Load testing tools and setup guidelines &#123;#testing-tools&#125;

There are certain tools that pair best depending on what development platform you use for load testing. The following are common platforms and their common load testing tools:

| Development Platform | Tool |
| --- | --- |
| Java | [JMeter](#jmeter) |
| Python | [Locust](#locust) |

To conduct effective load testing, you will need the following tools: InfluxDB, Grafana, and JMeter/Locust. Below are the steps to set up each component:

#### InfluxDB: &#123;#influxdb&#125;

InfluxDB is a time-series database that works seamlessly with JMeter and Grafana. To install InfluxDB, visit [InfluxDB download](https://portal.influxdata.com/downloads/).

**Warning:** Due to known issues with the 2.x version of InfluxDB, you need to download the 1.x version of InfluxDB. You can find the 1.x version if you follow the download link above and then scroll down to the bottom of the webpage and select **Are you interested in InfluxDB 1.x Open Source?**.

#### Grafana: &#123;#grafana&#125;

Grafana is an open-source analytics and monitoring solution that supports various databases. The most commonly used approaches involve building Grafana dashboards using InfluxDB and Prometheus. This section focuses on the integration with JMeter and InfluxDB. To install Grafana, please visit the [Grafana download](https://grafana.com/grafana/download?pg=get&amp;plcmt=selfmanaged-box1-cta1&amp;platform=mac) page.

#### JMeter: &#123;#jmeter&#125;

JMeter is a powerful open-source load testing tool. To install JMeter, follow the installation guide available at the [JMeter website](https://jmeter.apache.org/download_jmeter.cgi).

#### Locust: &#123;#locust&#125;

Locust provides a flexible and efficient way to simulate user behavior and generate load on your system. Download the [load test zip file](https://developers.facebook.com/micro_site/url/?click_from_context_menu=true&amp;country=IE&amp;destination=https%3A%2F%2Fdevelopers.facebook.com%2Fresources%2FLoadTest-py.zip&amp;event_type=click&amp;last_nav_impression_id=1KLU92oUeSpUvNoBF&amp;max_percent_page_viewed=55&amp;max_viewport_height_px=1009&amp;max_viewport_width_px=1792&amp;orig_http_referrer=https%3A%2F%2Fdevelopers.facebook.com%2Fdocs%2Fwhatsapp%2Fcloud-api%2Fguides%2Fload-testing&amp;orig_request_uri=https%3A%2F%2Fdevelopers.facebook.com%2Fajax%2Fdocs%2Fnav%2F%3Fpath1%3Dwhatsapp%26path2%3Dcloud-api%26path3%3Dguides%26path4%3Dload-testing&amp;region=emea&amp;scrolled=true&amp;session_id=1A5EfT0NY2fOb0sOj&amp;site=developers) provided by Meta and extract it.

## Load testing using JMeter with InfluxDB and Grafana &#123;#using-jmeter&#125;

Once you have installed the necessary components, follow these steps below to configure your setup using JMeter:

To start **JMeter**, follow these steps:

1. Download JMeter using the provided download link.
2. Open the terminal and navigate to the JMeter folder.
3. Browse to the &quot;bin&quot; folder within the JMeter directory.
4. Run the following command to start the JMeter application: `./jmeter`

By executing this command, JMeter will launch, and you will have access to the JMeter application interface, allowing you to create and configure your performance tests.

Next step is to **import the test plan**. To import a sample test plan into JMeter, follow these steps:

- Download the zipped test plan file provided and extract its contents into the JMeter folder. This file contains the necessary configurations, and you only need to modify the variables.

- Open JMeter and navigate to the `LoadTest.en_US.jmx`  test file. You can do this by selecting **File** &gt; **Open** and locating the file within the JMeter interface.

- Once the test file is opened, JMeter will display the test plan structure. This imported test plan serves as a starting point, and you can modify it according to your specific requirements.

- Locate the &quot;HTTP Request&quot; element within the test plan. Here, you need to provide the necessary details in the POST body data, including the template object.

- Find the &quot;User Parameters&quot; element and add values for the following variables:

- **phoneNumberID**: this will be the test phone number ID provided by Meta

- **systemUserAccessToken**: this will be the access token also provided by Meta

- To configure InfluxDB for result collection, go to the &quot;Backend Listener&quot; element. Configure it to match the settings shown in the provided screenshot or documentation.

By following these steps, you will successfully import the sample test plan into JMeter and make the necessary modifications to tailor it to your specific testing requirements. This test plan provides a starting point for load testing your system and collecting performance metrics using JMeter and InfluxDB.

Next start the InfluxDB server and create a new database for storing JMeter readings:

- Start the InfluxDB Server:

- Open a terminal and navigate to the InfluxDB installation directory. In most cases, it can be found at **InfluxDB_folder/usr/bin**.

- Run the following command to start the InfluxDB server on port 8086:

```https
./influxd
```

- Create a New Database:

- Open a new terminal tab or window.

- Set the directory to the InfluxDB installation directory, typically located at **InfluxDB_folder/usr/bin**.

- Launch the InfluxDB console by executing the following command:

```https
./influx
```

- Create the Required Database:

- Within the InfluxDB console, create the necessary database for the JMeter configuration. In this example, let&#039;s name the database &quot;demo&quot;.

- Run the following command to create the database:

```https
create database demo
```

With these steps completed, the InfluxDB server will be up and running, and you will have a designated &quot;demo&quot; database ready to store the JMeter readings.

To set up Grafana for JMeter, follow these steps:

- Grafana Server:

- Open a new tab in the terminal and navigate to the Grafana installation directory. Typically, it can be found at **grafana_folder/bin**.

- Run the following command to start the Grafana server:

```https
./grafana-server
```

- This will start the Grafana server on the default port 3000. Ensure that other servers using port 3000 (such as Docker or NodeJS) are turned off to prevent conflicts.

- Grafana Client:

- Open a web browser and visit [http://localhost:3000](http://localhost:3000). You will be prompted to log in to Grafana.

- Use the following login credentials:

```https
Username: admin
Password: admin
```

- Once logged in, hover over the gear icon (configuration) located at the bottom-left corner and select **Data Sources** to access the list of data sources. Select **InfluxDB** from the available options to open the InfluxDB interface.

- Configure the InfluxDB settings:

- Set the Query Language to **InfluxQL**.

- Enter the InfluxDB URL as [http://localhost:8086](http://localhost:8086) (the URL used to access InfluxDB).

- Choose the previously created **demo** database as the target database for JMeter readings.

- Click on **Save and Test** to save the InfluxDB configuration and verify the connection.

- To set up the Grafana dashboard, hover over the square icon on the left side of the Grafana app and select **+Import**.

- Load the Grafana dashboard with ID **5496** by clicking on **Load**.

With these steps completed, the Grafana configuration is now finished, and you are ready to run your JMeter test. Ensure that both JMeter and InfluxDB are running, and then execute the JMeter test to populate the Grafana dashboard you have just set up.

### Running the test in JMeter &#123;#running-jmeter&#125;

Once you have completed the setup steps outlined in the previous section, follow these instructions to run the test using JMeter:

1. Navigate to the **Thread Group** element in your JMeter test plan. This is where you configure the test settings.
2. Specify the desired number of threads or users that will simulate concurrent access to your application. Each thread represents a virtual user. The number of threads directly impacts the load on your system.
3. Set the ramp-up period to define how JMeter gradually increases the number of users from zero to the desired total. The ramp-up period is measured in seconds and determines the time it takes for JMeter to start all the specified users and reach the maximum number of users. For example, if you have 1000 users and a ramp-up period of 100 seconds, JMeter will start a new user every 0.1 seconds (1000 users / 100 seconds) until all 1000 users are active.
4. Once you have configured the test with the appropriate number of threads and the ramp-up period, you can proceed to start the test. Click on the start button, represented by a green play button, to initiate the load test.

## Load testing using Locust with InfluxDB and Grafana &#123;#using-locust&#125;

Once you have installed the necessary components, follow these steps below to configure your setup using Locust.

To start Locust, follow these steps:

- Download the [load test zip file](https://developers.facebook.com/resources/LoadTest-py.zip) provided by Meta and extract its contents.

- Open the `config.json`  file included in the extracted files. In this file, you need to specify the template, auth_token, and phone_number_id values to be used for the load test.

- Modify the JSON structure as follows:
```json
&#123;
    &quot;template&quot;: &#123;
        &quot;name&quot;: &quot;welcome_user&quot;,
        &quot;language&quot;: &#123;
            &quot;policy&quot;: &quot;deterministic&quot;,
            &quot;code&quot;: &quot;en&quot;
        &#125;
    &#125;,
    &quot;auth_token&quot;: &quot;&lt;YOUR_TOKEN&gt;&quot;,
    &quot;phone_number_id&quot;: &quot;&lt;YOUR_PHONE_NUMBER_ID&gt;&quot;
&#125;
```

- Replace `&lt;YOUR_TOKEN&gt;`  with the appropriate authentication token and `&lt;YOUR_PHONE_NUMBER_ID&gt;`  with the corresponding phone number ID.

- Ensure that Locust is installed on your system. If not, install it by running the following command:

```https
pip3 install locust
```

- Verification:

- Verify that Locust has been successfully installed by running the following command:
```https
locust -V
```

- This will display the installed Locust version.

- Install the InfluxDB listener/connector to enable loading Locust test results directly into InfluxDB:

- Use the following command to install the listener:
```https
pip3 install locust-influxdb-listener
```

- Installing the listener allows you to seamlessly integrate Locust with InfluxDB for storing and analyzing load test results.

Next start the InfluxDB server and create a new database for storing Locust readings:

- Start the InfluxDB server:

- Open a terminal and navigate to the InfluxDB installation directory. In most cases, it can be found at **InfluxDB_folder/usr/bin**.

- Run the following command to start the InfluxDB server on port 8086:
```https
./influxd
```

- Create a new database:

- Open a new terminal tab or window.

- Set the directory to the InfluxDB installation directory, typically located at **InfluxDB_folder/usr/bin**.

- Launch the InfluxDB console by executing the following command:
```https
./influx
```

- Create the required database:

- Within the InfluxDB console, create the necessary database for the JMeter configuration. In this example, let&#039;s name the database `pyt` .

- Run the following command to create the database:
```https
create database pyt
```

To set up Grafana for Locust, follow these steps:

- Grafana Server:

- Open a new terminal tab and navigate to the directory where Grafana is installed (**grafana_folder/bin**). Run the following command to start the Grafana server:
```https
./grafana-server
```

- This will initiate the Grafana server on the default port 3000. Ensure that any other services utilizing port 3000 are stopped or turned off.

- Open a web browser and go to http://localhost:3000. Use the following login credentials to access Grafana:
```https
Username: admin
Password: admin
```

- On the Grafana web interface, hover over the gear icon (configuration) located on the bottom left corner. Then select **Data Sources** to view a list of data sources.

- Select **InfluxDB** to open the InfluxDB interface. In the InfluxDB interface, configure the following settings:

- Query Language: Select **InfluxQL**

- URL: Set the URL as [http://localhost:8086](http://localhost:8086)

- Database: Set the database as `pyt`

- Click on &quot;Save and Test&quot; to save the configuration and verify the connection to InfluxDB.

- To set up the Grafana dashboard:

- Hover over the four-square icon on the left side of the Grafana interface and select **+Import**.

- Click on **Upload JSON file** and upload the `locust-grafana-dashboard.json`  file [provided by Meta](#using-locust) in the zip archive.

- Click **Load** to import and load the Locust Dashboard with its pre-configured settings and visualizations.

By following these steps, you will have successfully set up Locust for load testing and ensured the necessary dependencies and configurations are in place.

### Running the test in Locust &#123;#running-locust&#125;

Once you have completed the setup steps outlined in the previous section, follow these instructions to run the test using Locust:

- Navigate to the folder where you installed the load test zip file.

- Open your terminal or command prompt and run the following command to start the Locust server on port 8089:
```https
locust -f locustFile.py
```

- After running the command, open your web browser and go to [http://localhost:8089/](http://localhost:8089/). This will open the Locust web interface.

- In the Locust web interface, click on **New Test** to configure your test.

- Specify the desired number of users for your load test. These users represent the simulated virtual users or clients that generate traffic to your application during the test. The number of users determines the amount of concurrent traffic generated.

- Set the spawn rate, which determines the rate at which new virtual users are created during the load test. It controls how quickly the number of concurrent users ramps up. For example, if you set a spawn rate of 10 users per second, Locust will start with a few users and gradually increase the number of active users by 10 each second until the desired number of users is reached.

- Take into consideration the optimal user-to-throughput ratio for your test. As a general guideline, you can add approximately 100 users for every 300 messages per second (MPS) throughput. Keep in mind that each user takes approximately 350ms to compute.

- Ensure that the host field is set to the appropriate value, such as [https://graph.facebook.com](https://graph.facebook.com), depending on the system you are testing.

- Click on **Start Swarming** to initiate the load test.

In the Locust terminal console, you should see verbose output indicating that the load test is running. Monitor the console for any reported failures. If there are no failures reported, it indicates that your initial test is successful, and you can proceed to configure the Grafana Dashboard.
