# chattag
Insight Data Science project

   <h2>Why detect sentiment of Twitch chat?</h2>
        <h3>What is Twitch?</h2>
	        <p>Twitch is an online live video streaming platform. It started in 2011 as a branch of Justin.tv which is a broadcasting site streaming people's everyday life. Since the aquisition by Amazon in 2014, Twitch has been primarily focusing on video game streaming. With the booming growth in the last 4 years Twitch has made 1.7 billion revenue in 2017 and possesses active users of 15 million on a daily basis. Now Twitch has become the world's leading social video service and community for gamers.</p>
        <h3>Live Chat</h3> 
                <p>In addition to choosing from millions of live channels, the viewers are able to express their opinions in real time as well. The interaction between the host and viewers is an essential component to the Twitch viewing experience, which is remarkably different from watching pre-recorded videos provided by YouTube. Open up any Twitch channel, you will find the world of the video games takes the center of the screen, while scrolling comments and chats from the viewers are flying through the screen on the right side. </p>
		
 ![screenshot](https://user-images.githubusercontent.com/30357662/42741154-ec93c7f4-887d-11e8-93b3-6c9cc862d832.png)
 
 <p><br>As a streamer, if your video content is popular and appreciated by viewers, you can join Twitch's Partner Program which will give a share of viewer's subscription fee when they signed up for your channel. Therefore, viewer's opinion and fondness towards the video content is crucial to streamers who's trying to make a living via this platform. Live chat is a great source to distill viewer's opinion, however it can be labor-intensive to read through the chat log manually. Therefore, my goal is to build a tool to detect the real time sentiment of the Twitch chat. </p>
	 <hr>
         <h2>Approach </h2>
	 <p>Different from other contextual languages, around half of the Twtich chat has at least one emoticons. Here I'm going to tag the sentiment of the text and emoticons separately. </p>
         <h3>Sentiment of text</h3>
	 <p>To detect the polarity of sentiment in text, there's existing natural language processing tools that I can directly apply. Here I used NLTK.VADER (Valence Aware Dictionary for sEntiment Reasoning) to tackle this problem. <br></p>
<p>This algorithm is a lexicon and rule-based method. It takes an entire sentence and returns four scores. These scores represent the sentence's negativity, neutrality, positivity and a combined score of the three. This powerful tool also combines lexical features such as adverb, punctuations and capitalization into consideration of sentiment intensity.</p>

![nltk](https://user-images.githubusercontent.com/30357662/42741156-ef714d3e-887d-11e8-92e9-d1cc6f30819d.png)

<h3>Sentiment of emoticons</h3>
         <p><br>Obtaining the sentiment label of emoticons is the core of this problem. First of all, there's no existing library or lexicon for the Twitch emotes, because this problem hasn't been looked at by others before. I have found a literature (Seniment of Emojis) built a emoticon lexicon using Twitch chat. However, the sentiment of the emojis is computed from the sentiment of the tweets in which they occur. This assuamption doesn't necessarily hold with the Twitch chat. <br>How do I find the sentiment label of emoticons then? In the process of solving this problem, I encounter a lot of emoticons that I'm not sure what they mean and how to use them. Therefore, I need to refer to the community guide's description from Reddit or Stream Community. By reading the description, I found that the words used for describing the emoticon can simulate its sentiment. Therefore, I run NLTK.VADER on the emoticon's description and use the sentiment score of the description to approximate the sentiment of the emoticon. I was able to find descriptions from top 40 commonly used emoticons, which accounts for 90% occurance in total. 
	</p>
  
![emotes](https://user-images.githubusercontent.com/30357662/42741165-f446e706-887d-11e8-9f50-d684bdf8e3e6.png)

  
   <h2>Building a machine-learning model</h2>
         <p>Now that we have sentiment features from both text and emoticons, we need to build a machine-learning model to weigh them and combine them together into a sentiment score. There is a wide range of algorithms to choose from, and I ended up trying logistic regression, support vector machine (SVM, with rbf kernel) and gradient boost tree classifier. Each of these algorithms come from a slightly different aspect to do the job. Logistic regression is the representation of classic linear regression model, SVM is adaptive to the non-linear classification problem, while the tree-based algorithm is an ensemble approach to learn from existing labels. Here is some results of these models:</p>
	<center>
            <img src="../../static/images/rocs2.png" alt="rocs" width="600">
        </center> 
	<p><br>The metric I used to choose my model is the area under the receiver operating characteristic curve (AUC of ROC). This is a good way to deal with class imbalance and values both recall and precision equally. The figure above shows AUC of ROCs of the testing set with two separate modeling. The first model only uses sentiment features of text and the classification capability is not that great. However, the performance of the model is significantly boosted when the sentiment features of emoticons are incorporated.</p>


   <hr>
        <h2>Insights</h2>
	<p>Here, I built an algorithm to predict the sentiment of Twitch based on the text and emoticons. There are a few things I learned from this study. </p>
	<p>First, the sentiment of text is not as important as the sentiment of emoticons in the context of live chat. </p>
	<p>Second, even though the state-of-the-art tools are really powerful, but they are not applicable to every scenario. 
