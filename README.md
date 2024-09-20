                         
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
> Currently Movio has `three` / `3` microservices. 
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

<details>
<summary><h1 align="center">Key Images of Movio Workflow</h1></summary>

**In this section, I am adding some `key images` of `Movio workflow`.**

<br/> 

* A snapshot of Movio Player with Multiple Subtitle Support

![movio-player-with-subtitle](https://github.com/user-attachments/assets/548e912d-d15a-4ee5-8d3c-5dc1cf563b23)

<br/> 

* The `MPEG-DASH` Segments are being served through a `CDN` (Gcore CDN)
> The `CDN URL` is `movio-cdn.algocode.site`. 
> 
>> I am using my old domain I purchased for my another project - <a href="https://github.com/Mahboob-A/algocode/">Algocode</a>. This is a microservices backend solution of `Online Judge` just like `Leetcode`. No other `3rd Party APIs` has been used. User passes code in `C++`, and the  `RCE Enginer` executes the code in a secure docker container and generates results such as `AC`, `WA`, `TLE` etc. To learn more about `Algocode`, please visit  <a href="https://github.com/Mahboob-A/algocode/">Algocode</a> here. 

![segments-served-from-cdn](https://github.com/user-attachments/assets/18f3760b-cea9-4b4f-9fda-515e66ab0076)

<br/> 

* Segments in `S3 Bucket` 

![segments-in-s3-bucket](https://github.com/user-attachments/assets/d87ca384-b53c-4723-9a44-07ccc62f4430)

<br/> 

* Subtitles in `S3 Bucket` 

![subtitles-in-s3-bucket](https://github.com/user-attachments/assets/41552ff6-ce4c-4b3a-ac7a-eda1afb0df9e)

<br/> 

* `AWS Lambda` function

![aws-lambda-function](https://github.com/user-attachments/assets/3c992e40-0bb9-4aeb-bc55-303364521bc0)

<br/> 

* `DASH Segmentation` with `Adaptive Bitrate` of the Video

![dash-segments-with-abr](https://github.com/user-attachments/assets/01ce799f-2d64-4b7d-ab88-d118e36f2210) 

<br/> 

* `DASH Segments` Batch Processing to Upload in `S3 Bucket` 


![dash-segments-s3-batch-processing](https://github.com/user-attachments/assets/7d999a6f-8edd-4367-acfb-71e0be92ffdd)

<br/> 


<br/> <br/>

</details>

<details>
<summary><h1 align="center">How to Run Locally</h1></summary>

* In order to watch the video file, you do not need to run any of the services! 

* As all the segments are uploaded in `S3 Bucket`, and I have already set up Gcore CDN in order to serve the `DASH `Segments`, you only need to run a `html` page which has a `dash player` in it. 

* Please copy this already available `dash player` from the  <a href="https://github.com/Mahboob-A/Movio-API-Service/">Movio API Service </a>, and run it with the `Live Server`, and you will be able to see the video with `subtitle!`

* Please <a href="https://github.com/Mahboob-A/Movio-API-Service/blob/main/src/html-templates/dash-plyaer-current.html/">COPY THIS HTML FILE </a> , and put the `long video` or `short video` `CDN URL` in the `url` variable, and depending upon the long or short video URL, the video will be played. 

* **If you want to setup local development, you need a few AWS credentials as well as RabbitMQ Credential, Gcore CDN Credential, CloudFlare DNS** 


<br/> <br/> 

</details>
<details>
<summary><h1 align="center">Watch In Action | YouTube Video</h1></summary>


## Watch Movio on Action 

<br>

#### Video 01: Watch The General Video Playback Demonstration Of Movio 

* In this video, you can watch the general video playback of Movio with Subtitles. 


<a href="https://youtu.be/F_R2KIXYBMU" target="_blank">
  <img src="https://img.youtube.com/vi/F_R2KIXYBMU/0.jpg" alt="Watch the video">
</a>

<br>
<br>


#### Video 02: Movio High Level Architecture Understanding 

* In this video, you can learn more about the high level architecture of Movio

TimeStamp: 

A. Introduction: 00:00

B. Movio Auth Service: 02:05

C. Movio API Service: 03:55 

D. Movio Worker Service: 09:30 

E. Movio Worker Service Celery Pipeline: 11:40 

<a href="https://youtu.be/Y2_KYdhKGhw" target="_blank">
  <img src="https://img.youtube.com/vi/Y2_KYdhKGhw/0.jpg" alt="Watch the video">
</a>

<br>
<br>

#### Video 03: Watch the Big Picture of Movio - How Movio <a href="https://github.com/Mahboob-A/Movio-Worker-Service/">Movio Worker Service</a> and <a href="https://github.com/Mahboob-A/Movio-API-Service/">Movio API Service</a> and the Celery Pipeline Works Internally 

TimeStamp: 

A. Introduction: 00:00

B. Docker Compose Introduction: 02:10 

C. Django Middleware for Auth and Video Body Check: 03:55 

D. Docker Up Movio API Service: 08:57

E. Show Movio API Service Celery Logs: 11:24

F. Run Django Management Command for API Service: 11:50 

G. Docker Up Movio Worker Service: 12:40 

F. Show Flower for Celery Monitoring: 13:50 

G. Show Movio WOrker Service Celery Logs: 14:30 

H. Run Django Management Command for Worker Service: 15:00 

I. Submit Video to Movio API Service from Postman [POST]: 19:15 

J. Celery Task Explanation of Movio API Service: 19:50 

K. API Service Management Command for RabbitMQ: 21:10 

L. Worker Service Flower: 23:30 


Worker Service Celery Task: 

M. Worker Service Celery Tasks: 24:00 

O. Worker Service Celery Task - Download Video from S3: 24:43 

P. Worker Service Celery Task - Delete Video from S3: 25:50 

Q. Worker Service Celery Task - Extract Subtitle from Video: 27:40 

R. Worker Service Celery Task - Upload Subtitle to Translate for AWS Lambda: 29:40 

S. Worker Service Celery Task - Transcode Video to MP4: 32:25 

T. Worker Service Celery Task - DASH Segment Video: 34:55 

U. Worker Service Celery Task - Edit Manifest to Add Subtitle Information: 39:00 

V. Worker Service Celery Task - Upload DASH Segments to S3 Entrypoint and Celery Callback: 39:40 

W. Worker Service Celery Task - Sub Task Batch Processing (Celery Chain, Group, Chord): 40:15

X. Worker Service Celery Task - Upload Segments Sub Task (S3 Batch Upload): 46:40 

Y. Worker Service Celery Task - MQ Message Publish: 48:15 


Movio API Service APIs:
 
Z. Get Singel Video Metadata Information: 49:30 

a. Search Video Title/Description/Subtitle Search #Elastisearch: 51:40 


AWS Services: 

b. AWS S3 for Segments and Subtitles: 54:18

c. Show DASH Manifest File with Subtitle: 55:10 

d. Show English and Bengali Subtitle: 56:00 

e. AWS Lambda Function for Subtitle Translation: 56:50 


VIDEO PLAY: 

See The Video being Served through Gcore CDN: 

f. See the Video Segments are Played: 58:00 

g. Run Live Server with DASH Player: 58:15 

h. See the Video is being Played: 59:00 

i. See a Long Video being Played with Seek Backward and Forward with Synked Subtitles through CDN: 01:01:20


<a href="https://youtu.be/hGbIbQ6gXwI" target="_blank">
  <img src="https://img.youtube.com/vi/hGbIbQ6gXwI/0.jpg" alt="Watch the video">
</a>


<br>
<br>


</details><details>
<summary><h1 align="center">Learnings and Challenges</h1></summary>


## Learning and Challenges 

* It was super fun to build this project. I learnt so much new concepts during building this project. 

* I begun the project development on 09/09/24, and developed till 19/09/24. It took total 10 days to build the project what it is right now. Few times, I thought to give up, but I knew, every problem comes with a solution, we just need to pay attention to the problem itself to understand the solution. 

* I am hoping to continue this project and add some more extensions such as send email or whatsapp message to the user, add  rate limiting to the video submission API with Token Bucket or Fixed Window or Dynamic Window algorithm, add few more middleware to decrease the load to the view as I am already using to middleware to authenticate the request and validate the video upload API. 

 <br/> <br/> 


</details>
<br/>

<a href="https://www.linux.org/" target="blank">
<img align="center" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/linux/linux-original.svg" alt="Linux" height="40" width="40" />
</a>
<a href="https://postman.com" target="blank">
<img align="center" src="https://www.vectorlogo.zone/logos/getpostman/getpostman-icon.svg" alt="Postman" height="40" width="40" />
</a>
<a href="https://www.w3schools.com/cpp/" target="blank">
<img align="center" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/cplusplus/cplusplus-original.svg" alt="C++" height="40" width="40" />
</a>
<a href="https://www.java.com" target="blank">
<img align="center" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/java/java-original.svg" alt="Java" height="40" width="40" />
</a>
<a href="https://www.python.org" target="blank">
<img align="center" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/python/python-original.svg" alt="Python" height="40" width="40" />
</a>
<a href="https://www.djangoproject.com/" target="blank">
<img align="center" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/django/django-original.svg" alt="Django" height="40" width="40" />
</a>
<a href="https://aws.amazon.com" target="blank">
<img align="center" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/amazonwebservices/amazonwebservices-original-wordmark.svg" alt="AWS" height="40" width="40" />
</a>
<a href="https://www.docker.com/" target="blank">
<img align="center" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/docker/docker-original-wordmark.svg" alt="Docker" height="40" width="40" />
</a>
<a href="https://www.gnu.org/software/bash/" target="blank">
<img align="center" src="https://www.vectorlogo.zone/logos/gnu_bash/gnu_bash-icon.svg" alt="Bash" height="40" width="40" />
</a>
<a href="https://azure.microsoft.com/en-in/" target="blank">
<img align="center" src="https://www.vectorlogo.zone/logos/microsoft_azure/microsoft_azure-icon.svg" alt="Azure" height="40" width="40" />
</a>
<a href="https://circleci.com" target="blank">
<img align="center" src="https://www.vectorlogo.zone/logos/circleci/circleci-icon.svg" alt="CircleCI" height="40" width="40" />
</a>
<a href="https://nodejs.org" target="blank">
<img align="center" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/nodejs/nodejs-original-wordmark.svg" alt="Node.js" height="40" width="40" />
</a>
<a href="https://kafka.apache.org/" target="blank">
<img align="center" src="https://www.vectorlogo.zone/logos/apache_kafka/apache_kafka-icon.svg" alt="Kafka" height="40" width="40" />
</a>
<a href="https://www.rabbitmq.com" target="blank">
<img align="center" src="https://www.vectorlogo.zone/logos/rabbitmq/rabbitmq-icon.svg" alt="RabbitMQ" height="40" width="40" />
</a>
<a href="https://www.nginx.com" target="blank">
<img align="center" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/nginx/nginx-original.svg" alt="Nginx" height="40" width="40" />
</a>
<br/>

#### 🔗 Links

[![Email Me](https://img.shields.io/badge/mahboob-black?style=flat&logo=gmail)](mailto:connect.mahboobalam@gmail.com?subject=Hello) 
  <a href="https://twitter.com/imahboob_a" target="_blank">
    <img src="https://img.shields.io/badge/Twitter-05122A?style=flat&logo=twitter&logoColor=white" alt="Twitter">
  </a>
  <a href="https://linkedin.com/in/i-mahboob-alam" target="_blank">
    <img src="https://img.shields.io/badge/LinkedIn-05122A?style=flat&logo=linkedin&logoColor=white" alt="LinkedIn">
  </a>
  <a href="https://hashnode.com/@imehboob" target="_blank">
    <img src="https://img.shields.io/badge/Hashnode-05122A?style=flat&logo=hashnode&logoColor=white" alt="Hashnode">
  </a>
  <a href="https://medium.com/@imehboob" target="_blank">
    <img src="https://img.shields.io/badge/Medium-05122A?style=flat&logo=medium&logoColor=white" alt="Medium">
  </a>
  <a href="https://dev.to/imahboob_a" target="_blank">
    <img src="https://img.shields.io/badge/Dev.to-05122A?style=flat&logo=dev.to&logoColor=white" alt="Devto">
  </a>
  <a href="https://www.leetcode.com/mahboob-alam" target="_blank">
    <img src="https://img.shields.io/badge/LeetCode-05122A?style=flat&logo=leetcode&logoColor=white" alt="LeetCode">
  </a>