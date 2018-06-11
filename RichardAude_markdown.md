
# Richard Aude - Markdown

This is my formatted Markdown text. I am going to write about my part of the presentation, and will add numerous other thoughts about possible **Digital Humanities** projects, as well as mentioning what I have already achieved in the Digital Humanities.

## The history and the basic intent of Markdown
Markdown was mainly developed by John Gruber, but Aaron Swartz made significant contributions regarding the Markdown syntax.
In March 2004, Gruber published a [blogpost](https://daringfireball.net/2004/03/dive_into_markdown) on his blog *Daring Fireball* where he wrote about his motivation to develop Markdown. 
 
![John Gruber](https://upload.wikimedia.org/wikipedia/commons/6/64/John_Gruber%2C_2009_%28cropped%29.jpg "John Gruber in 2009")

His basic idea was that writing for the world wide web should be more fun than writing in html, especially regarding paragraphs with just plain text in it. He called it a *pain in the ass* to escape special signs like the ampersand in html. For example, **AT&T** in html would be written like "AT\&amp;T", which can be quite annoying. Furthermore, Gruber wrote that he would like writing for the web to be just like writing e-mails, meaning that it is almost *wysiwyg*, and you don't have to painfully preview everything to check if it looks alright.

Gruber was accompanied by Aaron Swartz, who made an important contribution to Markdown in form of his [*atx-headers*](http://www.aaronsw.com/2002/atx/intro).

![Aaron Swartz](https://upload.wikimedia.org/wikipedia/commons/thumb/0/06/Aaron_Swartz_profile.jpg/346px-Aaron_Swartz_profile.jpg "Aaron Swartz in 2008")

These atx-headers basically introduced the idea of writing headers with one or more '#' characters, to indicate the size of the header. In addition, he also established the Markdown way of writing lists.
All in all, Swartz was a very interesting person, and he tragically committed suicide at a very young age. I recommend to at least check out the [article on wikipedia](https://en.wikipedia.org/wiki/Aaron_Swartz) about him, or to watch the documentary [*The Internet's Own Boy*](https://en.wikipedia.org/wiki/The_Internet%27s_Own_Boy).
## Usage
Markdown is widely used. The most important example of the usage of Markdown is **Github**, where it is required to write the readme-file of a repository in Markdown. But Markdown can be easily implemented in other blogposting websites with a technophilic audience as well, **Reddit** or **Stack Overflow** are just other prominent examples.

# What I've done in DH so far
In the 2017/18 winter semester I've worked on a project to assemble a textual corpus and to perform some operations on it. It was basically my first attempt to do anything in the field of digital humanities, therefore it was something which turned out to be quite more complex than I initially thought it would be. But I was just so fascinated with the idea that a corpus could be literally any piece of written text, let it be the inaugural addresses of all US-American presidents, or *Moby Dick*, or just some random chat history.
What I did was to assemble a corpus consisting of articles that were published on a certain day by two different German news outlets, *Spiegel Online* and *FOCUS Online*, and compared them regarding the complexity of the language that was used in the articles. Now this required a very precise definition of what I called 'complexity' of language.  My definition of complexity was mainly based on *easily-to-measure-data*, like counting the length of the word, or dividing the amount of unique words by the absolute amount of words that were written that day by either *Spiegel* or *FOCUS Online*. I applied my measuring methods on the output of these news outlets from six different days. The outcome of my examination was that on each of these days, *Spiegel Online* had a more complex language. This is not really a surprise, if you compare the reputations of these two news outlets, because *Der Spiegel* and *Spiegel Online* are internationally known for their investigative journalism, while *FOCUS Online* is just a news site that publishes 'clickbaity' articles about rather irrelevant topics, such as 
> 10 things you can do with aluminium foil

and the like. 
To illustrate the part of gathering data, I included the following code block, which, when executed, will generate one text file for each article that was published on January 27, 2018 on *Spiegel Online*:
``` python
from urllib.request import urlopen
from bs4 import BeautifulSoup
import re

html = urlopen("http://www.spiegel.de/nachrichtenarchiv/artikel-27.01.2018.html")
bsObj = BeautifulSoup(html, "lxml")
z = 0
for link in bsObj.find("div", {"class": "column-wide"}).findAll("a"):
    if 'href' in link.attrs:
        zweig = link.attrs['href']
        print(zweig)
        if not 'bento' in zweig:
            html2 = urlopen("http://www.spiegel.de"+zweig)
            bsObj2 = BeautifulSoup(html2, "lxml")
            headline1 = bsObj2.findAll("span", {"class": "headline-intro"})
            f = open('sponArticle'+str(z), 'w')
            for headline in headline1:
                f.write(headline.text+' ')
                f.flush
                f.close
                    #print(headline.text + ' ')
            headline2 = bsObj2.findAll("span", {"class": "headline"})
            for realHeadline in headline2:
                sameFile = open('sponArticle'+str(z), 'a')
                sameFile.write(realHeadline.text + ' HEADLINEENDSHERE ')
                    #print(realHeadline.text + ' ')
            intro = bsObj2.findAll("p", {"class": "article-intro"})
            if len(intro) > 0:
                getIntro = intro[0].findAll("strong")
                for schrift in getIntro:
                    myfile = open('sponArticle'+str(z), 'a')
                    myfile.write(schrift.text + ' INTROENDSHERE ')
                    myfile.flush
                    myfile.close
                        #print(schrift.text + ' ')
            allText = bsObj2.findAll("div", {"class": "article-section clearfix"})
            if len(allText) > 0:
                ps = allText[0].findAll("p")
                myfileAgain = open('sponArticle'+str(z), 'a')
                for text in ps:
                    myfileAgain.write(' ' + text.text)
                    myfileAgain.flush
                    myfileAgain.close
                        #print(text.text)
                    html2.close
        z=z+1
   ```

# Projects in DH which could be cool
As we discussed with Mr. Crane in the seminar on June 6, I could imagine a few projects that could be interesting to realise with the methods of the digital humanities and digital philology:
### Projects regarding local history
Leipzig as a city had a very *eventful* history, especially in the younger past, regarding the **German Democratic Republic** and the demonstrations around 1989, which led to the fall of the GDR-Regime. Around that I can think of:
* examinations around the events of 1989
	* to create an overview of the political statements that are now frequently citated, like the answer Günter Schabowski gave at the press conference on November 9th, that led to the fall of the Berlin Wall, about when the new travel regulations would come into effect (*"As far as I know — effective immediately, without delay."* German: _"Das trifft nach meiner Kenntnis … ist das sofort … unverzüglich."_) and where the citations are made
	* to do a network analysis of people at the time (how did the people organise the demonstrations, how did the *Ministerium für Staatssicherheit*, or *StaSi* for short, draw connections between single members of the 'resistance' and so on)
### Projects regarding language
* to analyse the reception of different events from different points of view
	* There is one example of a propaganda show in the GDR that was called [*Der schwarze Kanal*](https://en.wikipedia.org/wiki/Der_schwarze_Kanal), which used media from West German TV shows and tried to give a communist commentary on it. It would be interesting to analyse which parts often needed a communist commentary.
	* One could try and do a sentiment analysis how propaganda in the 20th century reported on different events. One interesting article that gave me the idea ist [this one](http://viewjournal.eu/television-histories-in-postsocialist-europe/the-eichmann-trial-on-east-german-television/) about the trial of the nazi lieutenant colonel Adolf Eichmann in Jerusalem in the 1960s.
	* to retrace the reception of nuclear power through the decades (maybe also by means of sentiment analysis)

Of course, all these suggestions are just pure thoughts that came to my mind while brainstorming, without considering things like copyright restrictions or big enough data sets. And, I am just a beginner in the whole field of digital humanities, with not very much experience on realistic projects. And some of my listed projects are ambitious, to say the least.

> Written with [StackEdit](https://stackedit.io/).
