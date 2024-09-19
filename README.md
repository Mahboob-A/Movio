                         
<br/>
<div align="center">
<a href="https://github.com/Mahboob-A/Movio/">
<img src="https://github.com/user-attachments/assets/e2ab9eff-401e-4951-8474-986981881842" alt="Logo" width="700" height="400">
</a>
<h3 align="center">Movio - A Scalable Video On Demand Platform in Microservices</h3>
<p align="center">
Movio is a VOD Platform Backend Solution Just Like Netflix!
<br/>
<br/>
<a href="https://github.com/Mahboob-A/Movio/"><strong>Read the blog »</strong></a>
<br/>
<br/>
<a href="https://github.com/Mahboob-A/Movio-Auth-Service/">Movio Auth Service .</a>  
<a href="https://github.com/Mahboob-A/Movio-API-Service/">Movio API Service .</a>
<a href="https://github.com/Mahboob-A/Movio-Worker-Service/">Movio Worker Service</a>
</p>
</div>
<h3 align="center">General Information </h3>

**Movio** is a _**High Performance**_ video streaming platform just like <a href="https://www.netflix.com/in/">_Netflix_</a> built in __Microservices__ Architecture. A viewer can signup in _Movio_ and watch their favorite videos as well as upload their video for others to watch. 

**Movio** is primarily tries to mimic the core backend of any established _VoD_ platforms, such as **Netflix**, **YouTube**. The backend of **Movio**  is built on `Django`.

**The _Movio_ platform is backed by the following core technologies** - 

    a. Django - The Backend
    b. Proven Celery Pipeline - For Asynchronous Video Processing  
    c. Postgres - As Database 

    d. FFmpeg - Extract Subtitles, Transcode, Segment Videos in MPEG-DASH with Adaptive Bitrate Streaming Technology
    e. Gcore CDN - Distribute the Content Globally with Low Latency

    f. AWS Translate -  Translate the Subtitle.
    h. AWS Lambda - Automate the Subtitle Translation Process 
    i. AWS S3 - Store the DASH Segments

    j. RabbitMQ - As Message Broker 
    k. Celery - As Asynchronous Task Manager 
    l. Flower - To monitor Celery Workers 
    m. Redis - As Celery Backend 
    
    n. Elastisearch - To power the searches of Video Metadata and Subtitles 
    o. Docker - To Containerized the solution 
    p. Cloudflare - As DNS Service 
    q. Nginx - As Proxy Manger 
    r. Gunicorn - As App Server 



#### *NOTE* 

**A. Containerized Solution**

> The project is fully containerized using docker. A `make docker-up` would run the full system locally provided the `.envs`. 

**B. Documentation**

> Please visit the documentation page `localhost:{{NGINX HOST PORT}}/doc/` for more information related to API documentation. 
> 
