---
layout: page
title: BTS youtube highlighter
description: 
img: assets/img/blog/BTS_youtube_highlighter/bts.png
importance: 1
category: fun 
---



**NOTE: It takes a while to wake up the sleeping heroku server** 

The website is [here](https://bts-highlights.herokuapp.com/)
<!-- 
<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glwxofj9pjj30uc0ahgtf.jpg" alt="highlight_scene" style="zoom:67%;" /> -->
<!-- ![image alt text](/assets/img/blog/BTS_youtube_highlighter/intro.png) -->

<img src="/assets/img/blog/BTS_youtube_highlighter/intro.png" alt="drawing" width="100%"/>



#### Features I want to share 

(Only because I am proud of myself..) 

* As the video plays, comments that correspond to that time will appear in the box on the right 

* Comment histogram shows where are the highlights of that video (based on number of comments)

* If you click on the bar in the comment histogram, it will take you to that part in that video. 

  

<!-- <img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glwyeiqc6lj31160u0191.jpg" alt="Screen Shot 2020-12-22 at 10.27.45 PM" style="zoom:67%;" /> -->
<!-- ![image alt text](/assets/img/blog/BTS_youtube_highlighter/pagesample.png) -->
<img src="/assets/img/blog/BTS_youtube_highlighter/pagesample.png" alt="drawing" width="100%"/>



#### Background

When there are interesting moments in a youtube video, users comment by indicating the specific time frame. 

<!-- <img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glbmwwptx6j30fa01o0st.jpg" alt="Screen Shot 2020-12-04 at 11.52.49 AM" style="zoom: 50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gldv2r1y60j30ey01m3yl.jpg" alt="Screen Shot 2020-12-06 at 10.06.18 AM" style="zoom: 50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gldv2zj20oj30y002ydgf.jpg" alt="Screen Shot 2020-12-06 at 10.06.35 AM" style="zoom: 50%;" />

<img src="https://tva1.sinaimg.cn/large/0081Kckwgy1gldv3ey4zoj30yc02oaaj.jpg" alt="Screen Shot 2020-12-06 at 10.06.47 AM" style="zoom: 50%;" />
 -->
<!-- ![image alt text](/assets/img/blog/BTS_youtube_highlighter/funny1.png)
![image alt text](/assets/img/blog/BTS_youtube_highlighter/funny2.png)
![image alt text](/assets/img/blog/BTS_youtube_highlighter/funny3.png)
![image alt text](/assets/img/blog/BTS_youtube_highlighter/funny4.png) -->

<img src="/assets/img/blog/BTS_youtube_highlighter/funny1.png" alt="drawing" width="50%"/>
<img src="/assets/img/blog/BTS_youtube_highlighter/funny2.png" alt="drawing" width="50%"/>
<img src="/assets/img/blog/BTS_youtube_highlighter/funny3.png" alt="drawing" width="100%"/>
<img src="/assets/img/blog/BTS_youtube_highlighter/funny4.png" alt="drawing" width="100%"/>




To me, this kind of comments look like valuable data. Specifically, I thought it would be great to use it for building a video highlight detection system. I decided to analyze BTS videos and ARMY's comments on Youtube. 



Some motivation and ideas of this project were  

* What kind of moments are ARMY's obsessed about? Are there similarities between popular scenes? 
* Would this motivate BTS members show more of what ARMY's want to see? 
* What are the trending slangs among the ARMY?
* Wouldn't this help make "funny" compilation videos? [Like this one ><](https://www.youtube.com/watch?v=hANoMuzGLRY)



#### Data 

All videos are from official BTS channels ([BANGTANTV](https://www.youtube.com/user/BANGTANTV) and [Big Hit Labels](https://www.youtube.com/c/BigHitLabels)). I collected videos from 3 playlists: [BTS festa](https://www.youtube.com/playlist?list=PL5hrGMysD_GvZPFPcdm9NCqKfQKPYrkFh), [BTS episode](https://www.youtube.com/playlist?list=PL5hrGMysD_Gt2ekpVt25B6C5ozZjVxQdh), and [Bangtan Bomb](https://www.youtube.com/playlist?list=PL5hrGMysD_Gu2a7-KuaQTxjRVPqM2Bi63).



Data collection process is as following:

1. Get all youtube videos of the selected playlists. 
2. For each video, use Youtube data API to download comments that contain the time stamp (ex. 00:45). 
3. Merge time stamps that are +- 3 seconds within each other.

I also wanted to include the official music videos, but each video had several thousand pages of comments, which quickly used up all my daily quotas of google API calls :'P



Some data statistics 

* Total number of videos : 429 
  * BANGTANTV BTS episode : 99
  * BANGTANTV BTS Festa : 57
  * BANGTANTV Bangtan Bomb : 273
* Average number of comments per video : 628.1
* Most commented scene has 1399 comments


#### Which moments did the ARMY's like the best?

"Cute" and "funny" are the most mentioned words. 

Popular scenes are mostly about members being funny and raw, which is in contrast to their charismatic stage performances. It's probably because people like to see others smile, laugh, have fun with each other and care for each other. In fact, many fans (including myself) say BTS brings joy in their life. I watch BTS videos, if I feel sad and stressed.



Here are some funny moments I found. 

**Playful BTS**

* [Crazy dance that is a preview for Dynamite?](https://www.youtube.com/watch?v=70SMjxn4FBA&t=217s)
* [Bunny Jungkook scaring Jin](https://www.youtube.com/watch?v=X89bYonOqaA&t=540s) 



**반전 매력 (Translates to unexpected charm..?)** 

* [Suga dancing](https://www.youtube.com/watch?v=V1i_x2_TGE0&t=259s)
* [Jungshook](https://www.youtube.com/watch?v=JeODXwSenKs&t=583s)




**Being professional**

* [Jungkook dancing](https://www.youtube.com/watch?v=eRkpkveBWyo&t=277s)

* [J-hope dancing](https://www.youtube.com/watch?v=eRkpkveBWyo&t=209s)

  

**Accidents** 

* [Suga accidentally hitting RM](https://www.youtube.com/watch?v=70SMjxn4FBA&t=125s)
* [Jimin's bloop during NYE NYC concert 2019](https://www.youtube.com/watch?v=UbibhcTvNlU&t=203s)



**Members doing something together (caring for each other, making fun of each other)**

* ["Shipsvkook"](https://www.youtube.com/watch?v=8ZbPyzh3pio&t=267s)

* [V slapping Jungkook](https://www.youtube.com/watch?v=o03lR8ErvL0&t=165s)

* [Vhope](https://www.youtube.com/watch?v=uxzxFPvUJAM&t=47s)

* [J-hope and JungKook](https://www.youtube.com/watch?v=o03lR8ErvL0&t=116s)

  

#### Keywords of all comments 

<!-- <img src="https://tva1.sinaimg.cn/large/0081Kckwgy1glwy14r802j30m80m8q8m.jpg" alt="wordcloud" style="zoom:67%;" />
 -->

![image alt text](/assets/img/blog/BTS_youtube_highlighter/keyword.png)

#### Sentiment analysis

I used [vaderSentiment](https://github.com/cjhutto/vaderSentiment) library to compute sentiment of all the comments collected. Non-english comments were translated to English. 

The sentiment score ranges from -1.0 to 1.0, referring to negative and positive, respectively. However, negative sentiment does not mean comments are bad. For example, if the comment is along the line of "crying because Suga" or "I am dying from cuteness 😭😭😭", then this will be computed as negative.

Also, this vaderSentiment library is trained on social media data, such as tweets and Rotten Tomato movie reviews. Positiviity bias (Human language reveals a universal [positivity bias](https://www.pnas.org/content/112/8/2389 ) may be why there are so many comments scoring ~0.23. 

![sentiment](https://tva1.sinaimg.cn/large/0081Kckwgy1gldy52j5trj30c0080749.jpg)



#### Website 

[Here is the link again](https://bts-highlights.herokuapp.com/)

p.s. Might be a bit slow on load, since Heroku keeps unvisited sites dormant after a while.



#### Conclusion

My favorite quote these days from RM

*"마음먹은 대로 살되 마음대로는 살지 않겠다"*



