                         
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

**C. Progress**
> I started to build the project on 09/09/24, and today is 190924. It's been 10 days since I was building the project in microservices. The project has `three` / `3` microservices. 
> I am constantly updating the ReadME, you may see different versions of Readme!
> 


**D. Future Updates**
> I am planning to add the following supports in the future Updates:
> 
>> i.  A Rate Limit to the video submission API implemented in `Token Bucket Algorithm` 
> 
>> ii. Implement Whatsapp Message to the User when the video processing is completed 
> 
>> iii. Send Email to the user when the video processing is completed. 
> 


<details>
<summary><h3 align="center">Movio Overall Workflow</h3></summary>

### Overall Workflow of Movio 

<br/><br/>


#### Workflow of <a href="https://github.com/Mahboob-A/Movio-Auth-Service/">Movio Auth Service </a>  

* The authentication system of `Movio Auth Service` is built from scratch. No `3rd party` packages has been used. 

* The Auth Service provides `JWT access and refresh` token with some additional custom user data encoded in it.


#### Workflow of <a href="https://github.com/Mahboob-A/Movio-API-Service/">Movio API Service </a> 

The <a href="https://github.com/Mahboob-A/Movio-API-Service/">Movio API Service </a> service is exposed to public access along with the <a href="https://github.com/Mahboob-A/Movio-Auth-Service/">Movio Auth Service </a> . Users can interact with  <a href="https://github.com/Mahboob-A/Movio-API-Service/">Movio API Service </a>  for video processing and requesting the video metadata. 

**NOTE:**  _See API Documentation Below for Full API Params and Information._

_**TL;DR;**_ 

**Upload API:**

* User send a `video file` along with  `title`, `duration`, and `description`. 
* The video is offloaded to `celery worker`. 
* The celery worker uploads the video to `S3 Bucket`, deletes the local file and creates a `RabbitMQ Event` with some data. 
* This  message is consumed by  <a href="https://github.com/Mahboob-A/Movio-WOrker-Service/">Movio Worker Service </a> to process the video. 

 **Stream API:**

* User passes a `video_id` and the API provides all the necessary information to play the video such as: `CDN URL`, and other video metadata. 

**Search APIs:**

* Search in a href="https://github.com/Mahboob-A/Movio-API-Service/">Movio API Service </a> is powered by `Elastisearch`. 
User can search a video by the below properties: 

> i. video title 
> 
> ii. video description 
> 
> iii. subtitle keyword 
> 

**List API:**

* The `ListView` is a paginated API. It responds with all the videos in the database with `paginated response`. 


##### Below I am describing below app wise how  <a href="https://github.com/Mahboob-A/Movio-API-Service/">Movio API Service </a> works internally. 

##### APP: Event Manager

* `event_manager` is the app that is responsible to communicate with the cloud `RabbitMQ` instance  <a href="https://github.com/Mahboob-A/Movio-API-Service/">Movio API Service </a> is using to communicate with  <a href="https://github.com/Mahboob-A/Movio-Worker-Service/">Movio Worker Service </a>. 

* The `event_manager` app consists of a `Django Management Command` that runs on different process to listen to the events of the Movio Inter Services. 

* It produces messages  to be consumed by  <a href="https://github.com/Mahboob-A/Movio-Worker-Service/">Movio Worker Service </a> and it also listens to the messages that are produced by  <a href="https://github.com/Mahboob-A/Movio-WOrker-Service/">Movio Worker Service </a>. 

* `Video Submission API` - `/api/v1/app/events/video-upload/` -> API that offloads the video processing to the `celery worker`.  

##### APP: Stream 

* The `stream` app is responsible for all the streaming related APIs. 

* `SingleVideoMetaDataAPI` - `/api/v1/app/stream/video-metadata/<uuid:video_id>/` ->  to get video metadata about a single video provided a `video_id`. 

* `ListView API`  - `/api/v1/app/stream/videos/all/`  -> API to get all the videos available in the platform with `paginated response.`

* The user needs to use a `dash player` and send a request to the `CDN URL` to play the video along with the `subtitle`.

##### APP: ES Search (Elastic Search)

* The app is responsible for managing all the `elasticsearch` related `documents`, `serializers` and `views`. 

* Movio is powered by `Elasticsearch` for searching in the platform. 

* `Search In Video MetaData` -  `/api/v1/app/search/video-metadata/`  -> To search on video `title` and video `description`. 

* `Search in Video Subtitle` - `/api/v1/app/search/subtitle/`  -> To search on video `subtitle`. 

<br/> 

</details>
