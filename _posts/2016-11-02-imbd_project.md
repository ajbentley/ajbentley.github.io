---

title: A Recipe for Net-success

---

### Summary
In this week's scenario we were tasked by Netflix to determine what factors contribute to a movie's popularity, using IMDB's ratings as a barometer.

The core problem is how to predict a movie's popularity in a way that it:

* is as resistant as possible to the changing tastes of the public

* accounts for the complex range of features a work like a film has

* is concise enough to be easily communicated and accepted


In order to achieve this it was necessary to start wide and narrow down. I gathered IMDB's information on their top 250 movies, including cast and crew, genre, release date, languages, awards won, and languages. Over 1,000 features were considered in creating this model.

After filtering through all of these, I ended with a short list of four items: Gross, Runtime, Metascore, and Month Released.


### Developing the Model

My first look at the topline quantitative data showed that one of the strongest predictors of whether a movie was popular on IMDB was its Metascore, though this was no guarantee.

Most movies were around 2 - 2 1/2 hours long and were released in either December or January.

It was also very clear that the rankings were strongly skewed towards more recent movies.

I was suspicious whether the preference for winter releases had changed significantly in recent years, but for expediency opted not to look deeply into whether that had changed.

I did take the skew in years to heart, however, and my first change to the data in the model was to determine the median year among the top 250 and cut it off. Because of this no movies released prior to 1991 were a part of the model.

Despite their predictive strength in the model, I excluded the number of votes on IMDB and the release year (the latter also because its real predictive power was leveraged in eliminating movies being considered). The votes were excluded since there was such correlation between them and the rating that it became clear that one was a function of the other and they were not independent (the more people rated a movie, the better it rated).

The initial MSE score before eliminating the earlier years and IMDB votes was 0.13. Afterwards it grew to a 0.72 <sup>1</sup>. Not what I'd hoped.

I ran a Random Forest Regressor that pulled the number back down to a 0.17. Giving it an AdaBoost brought it down another 2 points to a 0.15. Passing it through Gradient Boost, though, brought it up to a 0.21.

Since it was the best score, I took a look at what was driving the strength of the AdaBoost number. From the top 20 most significant drivers I chose to omit any specific people. There were two reasons for this.

The first is that there are people who have minor roles in great films. With no disrespect meant to William Sadler, it was not his performance as Heywood that made The Shawshank Redemption great, nor did his turn as Klaus Detterick make The Green Mile as popular as it was.

The second is that no one is consistently at the top of their game. Morgan Freeman's work in Evan Almighty didn't make that it any more watchable and Tim Robbins couldn't save Nothing to Lose.

If we were looking at the breadth of movies these actors were in it would make sense to include them, but as we're just looking at the top tier these make no sense.

In the end I opted to include the four strongest variables that were consistent through the iterations of this process:

Gross: Big budget movies are more likely to be hits.
Runtime: Movies should be over two hours but less than 3.
Metascore: Metascore is a ratings aggregation service.
Month Release: Movies were more likely to appear in the top 250 if they were released in December or January (even when excluding years prior to 1991)

### Histograms of key metrics

![](https://ajbentley.github.io/assets/images/imdb/imdb_hists_2.png?raw=true)


### PairPlots of key metrics

![](https://ajbentley.github.io/assets/images/imdb/imdb_pp.png?raw=true)


### Concerns

Just as alchemists have tried for centuries to change lead into gold, people have been seeking the recipe for what makes a popular movie (or TV show, play, song, comic book, etc.), generally leading to products that feel more like products than entertainment. Part of the reason for the success Netflix enjoys is that its primary criteria for whether an individual will like a movie is that individual's tastes.


### Future study

If the intention is to continue to rely on external data, the addition of budget and Rotten Tomato scores would be very useful. Even more informative would be to expand this from looking just at what appears in the top 250 but also what DOESN'T appear in it. In addition to the above examples, people remember Peter Jackson for the Lord of the Rings franchises, but forget that he was also responsible for Meet the Feebles, The Frighteners, and Dead Alive. His name does not guarantee popularity. A study comparing what appears in each quartile of the top 1000 would be much more useful than just looking at the top 250.

The best thing Netflix could do is leverage that individualism and get its clients to share their IMDB user names. They could then use NLP to match their reviews with other reviewers. This would allow Netflix to not only match on what movies they liked, but also on subtle things that show through written voice, such as education and passion for an actor or genre.


<sup>1</sup> MSE scores came back as negative, a mysterious quirk. They also changed between runs. Both are items to look into and update.
