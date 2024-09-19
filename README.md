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

**C. Progress**

> I started to build the project on 09/09/24, and today is 190924. It's been 10 days since I was building the project in microservices. I have learnt a lot during the stay. 
> I am constantly updating the ReadME, you may see different versions of Readme!
> 
