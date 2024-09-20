                         
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

**Please see the respective service for code review**

_**NOTE**: Currently I am updating the readme of  this central repository. The Readme update for each service might be delayed. This central repository will share the high level understanding of the **Movio Platform**. You are encourage to read throughly the **Movio Overall Workflow** Section below for high level understanding of **Movio Platform**._
 

**Movio Auth Service**: <a href="https://github.com/Mahboob-A/Movio-Auth-Service/">Movio Auth Service </a>  

**Movio API Service**: <a href="https://github.com/Mahboob-A/Movio-API-Service/">Movio API Service </a>

**Movio Worker Service**: <a href="https://github.com/Mahboob-A/Movio-Worker-Service/">Movio Worker Service .</a>

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
<summary><h1 align="center">Movio Overall Workflow</h1></summary>

## Overall Workflow of Movio 


### Workflow of <a href="https://github.com/Mahboob-A/Movio-Auth-Service/">Movio Auth Service </a>  

* The authentication system of `Movio Auth Service` is built from scratch. No `3rd party` packages has been used. 

* The Auth Service provides `JWT access and refresh` token with some additional custom user data encoded in it.

<br/>
<br/>

### Workflow of <a href="https://github.com/Mahboob-A/Movio-API-Service/">Movio API Service </a> 

The <a href="https://github.com/Mahboob-A/Movio-API-Service/">Movio API Service </a> service is exposed to public access along with the <a href="https://github.com/Mahboob-A/Movio-Auth-Service/">Movio Auth Service </a> . Users can interact with  <a href="https://github.com/Mahboob-A/Movio-API-Service/">Movio API Service </a>  for video processing and requesting the video metadata. The service `Produces RabbitMQ` events that would be consumed by <a href="https://github.com/Mahboob-A/Movio-Worker-Service/">Movio Worker Service </a> and the service also `Consumes RabbitMQ` events that are produced by <a href="https://github.com/Mahboob-A/Movio-Worker-Service/">Movio WorkerService </a>. 

Once <a href="https://github.com/Mahboob-A/Movio-API-Service/">Movio API Service </a> consumes a message that is produced by <a href="https://github.com/Mahboob-A/Movio-Worker-Service/">Movio Worker Service </a>, it updates the `status of the video processing` in the database. As an extension, I am planning to sending `Whatsapp Message and Email` to the user at this time to inform the User about the successful video process result.  

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


#### Below I am describing below app wise how  <a href="https://github.com/Mahboob-A/Movio-API-Service/">Movio API Service </a> works internally. 

 **APP: Event Manager**

* `event_manager` is the app that is responsible to communicate with the cloud `RabbitMQ` instance  <a href="https://github.com/Mahboob-A/Movio-API-Service/">Movio API Service </a> is using to communicate with  <a href="https://github.com/Mahboob-A/Movio-Worker-Service/">Movio Worker Service </a>. 

* The `event_manager` app consists of a `Django Management Command` that runs on different process to listen to the events of the Movio Inter Services. 

* It produces messages  to be consumed by  <a href="https://github.com/Mahboob-A/Movio-Worker-Service/">Movio Worker Service </a> and it also listens to the messages that are produced by  <a href="https://github.com/Mahboob-A/Movio-WOrker-Service/">Movio Worker Service </a>. 

* `Video Submission API` - `/api/v1/app/events/video-upload/` -> API that offloads the video processing to the `celery worker`.  

**APP: Stream** 

* The `stream` app is responsible for all the streaming related APIs. 

* `SingleVideoMetaDataAPI` - `/api/v1/app/stream/video-metadata/<uuid:video_id>/` ->  to get video metadata about a single video provided a `video_id`. 

* `ListView API`  - `/api/v1/app/stream/videos/all/`  -> API to get all the videos available in the platform with `paginated response.`

* The user needs to use a `dash player` and send a request to the `CDN URL` to play the video along with the `subtitle`.

**APP: ES Search (Elastic Search)**

* The app is responsible for managing all the `elasticsearch` related `documents`, `serializers` and `views`. 

* Movio is powered by `Elasticsearch` for searching in the platform. 

* `Search In Video MetaData` -  `/api/v1/app/search/video-metadata/`  -> To search on video `title` and video `description`. 

* `Search in Video Subtitle` - `/api/v1/app/search/subtitle/`  -> To search on video `subtitle`. 

<br/> 
<br/>

### Workflow of <a href="https://github.com/Mahboob-A/Movio-Worker-Service/">Movio Worker Service </a> 

The <a href="https://github.com/Mahboob-A/Movio-Worker-Service/">Movio Worker Service </a> is the heart of the Movio Platform. The service, just like the <a href="https://github.com/Mahboob-A/Movio-API-Service/">Movio API Service </a>, runs an additional `Process` `(Django Management Command)` to listen to the `RabbitMQ` events that are produced by <a href="https://github.com/Mahboob-A/Movio-API-Service/">Movio API Service </a>. The service also produces message after it finishes the video processing and that message is consumed by the <a href="https://github.com/Mahboob-A/Movio-API-Service/">Movio API Service </a>. 

Once the <a href="https://github.com/Mahboob-A/Movio-Worker-Service/">Movio Worker Service </a> consumes a message produced by <a href="https://github.com/Mahboob-A/Movio-API-Service/">Movio API Service </a>, a robust `Celery Pipeline` is activated. 

**Below, I am mentioning all the Celery tasks that are included in this Pipeline**.

**NOTE:** _All The Below Tasks Run Sequentially._

#### Task 01: Download Video From S3 

* This is the very first task in the pipeline. It downloads the video that was submitted by the user (which was uploaded to S3 by <a href="https://github.com/Mahboob-A/Movio-API-Service/">Movio API Service </a>). 

 
#### Task 02: Delete Video From S3 

* As the video is downloaded from S3 to process locally, this task deletes the video from S3 bucket as that video is no longer needed. 

#### Task 03: Extract Subtitle From Video 

* This task  make uses of `ffmpeg tool` and extracts the subtitle from the video, and saves it locally for further processing. 

#### Task 04: Upload Subtitle File to S3 for Lambda Processing

* This task uploads the `Subtitle` / `CC` file to a `S3` bucket. 

> **AWS Lambda Function for Subtitle Translation** 
> 
>> * The `S3` bucket is a trigger point for a AWS Lambda function. 
> 
>> The Lambda function downloads the subtitle file from S3, and translate the subtitle in `Bengali`, `Hindi`, `French` and `Spanish`. 
> 
>> The Lambda function uses AWS Translate Service to translate the subtitles. 
> 
>> Once the translation is completed, the Lambda function uploads all the subtitle files to another `S3 bucket` which is  designated for `production stage`. 
> 

#### Task 05: Transcode Video to MP4 

* As `MPEG-DASH` players are browser based, `dash player` can not directly serve `.mkv` container `dash segments`. 
Hence, it is necessary to `transcode` the video into `MP4` so that any `dash player` can play the segments. 

* This task make use of `ffmpeg tool` and transcode the video into `MP4` container format. 

* It uses `h264` codec for transcoding the video and `aac` for sound stream. 
 

#### Task 06: Make DASH Segments of the Video with Adaptive Bitrate 

* Once the `.MKV` video file is `transcoded` into `MP4` container type, it is ready for segmentation.    

* This task segments the video into chunks with adaptive bitrate technology. 

> **Adaptive Bitrate** 
> 
>> `Adaptive Bitrate (ABR) is very popular method of video segmentation. `ABR` makes video streaming very joyful as it accounts for the `network bandwidth` and `network latency` of the client. 
> 
>> If the client has slower network speed, the `ABR` technology downgrades the video quality automatically hence the client does not experience `buffering` or `minimal buffering`.
> 
>> In `ABR`, the segment size is automatically chosen by the `dash player`. 
> 
    
* The task segments the video in: `360p`, `480p`, and `720p` with `800 kbps`, `1200 kbps` and `2400 kbps` bitrate respectively. 


#### Task 07: Edit `manifest.mpd` File

* Once the video is processed for `DASH Segments`, it generates an `.mpd` file where all required information about the video is stored. 

* `ffmpeg` does not support adding `Subtitle` information in the `manifest.mpd` file, hence, we need to manually add the `Subtitle` information in the `manifest.mpd` file. 

* This task does the same job. It opens the `manifest.mpd` file, and adds some `AdaptionSet` in the `manifest.mpd` with the `BaseURL` of the `Subtitles`. 


#### Task 08: Upload DASH Segments to S3 and Chain Callback

* This task is interesting. This task creates `sub-tasks` within it to utilize the maximum resources of the server. 

* This task creates batch of a list of 10 segments, and a task task is created for each list. 

* Each list has 10 segments, hence, all the `sub-tasks` are dedicated to upload a batch of 10 segments in `S3`. 

> **Why Create Sub Task**
> 
>> I am creating sub-tasks in order to fully utilize the full throttle of the server. 
> Imagine, a video file has 1000 segments, and we are allocating only one `celery worker`, it is a bottleneck for two reasons: 
>
>>> i. The worker may fail as uploading straight 1000 segments may need much more connection time to the `s3_client`. 
>
>>> ii. Assigning only one worker doesn't utilizes the full capabilities of the server. Imaging a server has 8 workers. If we are assigning only one worker, it has high chances that a few worker may be idle/doing nothing. 
> 
>>> "We pay cloud providers for the cores. Cores are not promised to stay cool" - Someone Wise! 
> 
>>> Hence, in order to fully utilize all the capabilities and all the available workers, I am distributing the uploading task in smaller batches and allocating all the available workers to upload. 
> 
>>> This way, no worker needs to maintain a connection for long time, and as many available workers are working symultaniously, the upload is faster. 
> 


 * The task creates all the batches as `Celery Group` so that all the tasks may run symultaniously. 

* The task also creates a `callback chain` with two other tasks: 
> task i. publish message to mq 
> task ii. cleanup local files 

* Then the task creates a `celery cord` canvas `celery group` as the `header` and the `callback chain` as the callback so that 
the `cleanup local files` task only execute when all the uploading tasks are completed. 

#### Task 09: Publish MQ Message 

* This task is the first task in the previous `callback chain`. 

* It produces message for the  <a href="https://github.com/Mahboob-A/Movio-API-Service/">Movio API Service </a> with additional video metadata information so that the <a href="https://github.com/Mahboob-A/Movio-API-Service/">Movio API Service </a> may update the database. 

#### Task 10: Cleanup Local Files 

* This is the last task in the `callback chain` of the `callback` of the previous task. 

* This task clears all the processed data from local storage such as `Subtitle file`, `.MKV` and `.MP4` file and the local `DASH Segments`. 
 
<br/> <br/>

</details>

<details>
<summary><h1 align="center">Architecture of Movio  |  HLD </h1></summary>


## Architecture of Movio 

### Architecture of  <a href="https://github.com/Mahboob-A/Movio-Auth-Service/">Movio Auth Service </a>

  


![movio-auth-service-hld](https://github.com/user-attachments/assets/4354472a-78f8-4365-a3c5-1df6cf4a2982)



### Architecture of  <a href="https://github.com/Mahboob-A/Movio-API-Service/">Movio API Service </a>

![movio-api-service-hld](https://github.com/user-attachments/assets/8888cc08-1f82-4970-9b76-65d7d3b990ed)


### Architecture of  <a href="https://github.com/Mahboob-A/Movio-Worker-Service/">Movio Worker Service</a>

_**NOTE**_: <a href="https://github.com/Mahboob-A/Movio-Worker-Service/">Movio Worker Service</a> is only accessible through `Message Queue` Events. The `Nginx Container` in <a href="https://github.com/Mahboob-A/Movio-Worker-Service/">Movio Worker Service</a> is only for `Healthcheck` and for `Admin Portal`.  

![movio-worker-service-hld](https://github.com/user-attachments/assets/b9d41bf6-8c0f-4c75-bc35-f983c870d6de)

<br/> <br/>

</details>
