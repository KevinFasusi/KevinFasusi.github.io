---

layout: post
title: Naive Bayes Document CLassifier
excerpt: Enough with the Naive Bayes Classifiers already sheesh!
---

{% include header.html %}

# Naive Bayes Document Classifier PART 1-You did what? #

For my thesis, I am expected to write a literature review. The literature review is supposed to summarise the corpus of literature that covers my field of enquiry. The review has to be systematic, which in short means exhaustive and capable of being audited. 

To focus the literature search, you take keywords and create search terms. The search terms are then chained together to create search string. Depending on the database these search strings are then used to interrogate a journal database such as Science Direct, Proquest etc. 

To discriminate between irrelevant and relevant articles, a detailed exclusion criteria is used with hard criteria that excludes an article from being used. The criteria can be year of publication, peer-review etc. I found these easy to implement, they are objective and can in most cases be implemented using the filtering mechanism of the journal database being queried. A flow diagram detailing the process is illustrated in the [Business Research](http://www.amazon.co.uk/Business-Research-Practical-Undergraduate-Postgraduate/dp/0230301835/ref=sr_1_sc_1?ie=UTF8&qid=1437217747&sr=8-1-spell&keywords=business+research+jill+colis) book by Jill Collis. I was interviewed for this book and the case example is actually from my work. Dr Collis is an excellent lecturer and researcher in Accounting and it was an honour to be asked to participate.

However even after the application of the hard criteria some articles that pass this stage are not applicable to the literature.  Often after reading the abstract and maybe a few pages, I can decide on the articles relevance to my research. I wanted a quicker method, one that was systematic and scientific but also quick and if possible automated. I started to read around.

Early on in my reading I came across natural language processing and sentiment analysis. These topics lead me on to machine learning and then to a python library named NLTK. After further reading, I understood that using a trained copra, I could classify unknown text. So I took a look at python. I had used python before but for statistical calculations, for supply chain inventory planning and forecasting. I had knowledge of limited libraries and never worked with machine learning. For my efforts, I only managed to get a crap line chart for the word frequency in my training corpus. I am not being disparaging towards NLTK, my failure was all on me. My approach to learning the library was wrong. I even created my corpus by hand, compiling the text in one file.  I realised that I would have to take a step back.

I have experience working with VBA (please don’t judge me). I had written several applications and had increasingly focused on writing VBA like a true OOP language, which it is not. I later found out about a book called [Data Smart](http://www.amazon.co.uk/Data-Smart-Science-Transform-Information/dp/111866146X/ref=sr_1_1?ie=UTF8&qid=1437218510&sr=8-1&keywords=data+smart) by John W. Foreman. In the book Naive Bayes Classification is explained using tweets and excel. My first thought was, “This Guy!…” With all the anti excel sentiment out there, Mr Foreman was a breath of fresh air. Don’t get me wrong I do not advocate using the wrong tools for the job, but for comprehension of the subject matter this suited me just fine. Another [book](https://www.google.co.uk/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0CCIQFjAAahUKEwjb2t2py-TGAhWIbRQKHU0-A4k&url=http%3A%2F%2Fweb4.cs.ucl.ac.uk%2Fstaff%2FD.Barber%2Ftextbook%2F090310.pdf&ei=HDqqVdvcJojbUc38jMgI&usg=AFQjCNE1DaK9fD5mFcKXuRpSylvEP2wNsg&sig2=mZOo915kIYGLkWCaGfbZ_Q&bvm=bv.98197061,d.d24) by David Barber a Reader Information Processing in the department of Computer Science UCL, along with several articles and blogs helped me get a better understanding. Then I had a dangerous Idea. You know where I am going with this. I thought to myself “Why not write a Naive Bayes Classifier in VBA…[Why not!](https://youtu.be/V2rG8nKh4Cc?t=1m26s)” (think John Wilkinson the comedian when saying the why not! This dude is the voice in my head for some reason). I know this is blasphemy, why would I rewrite something that has been rewritten to death...Why not!

![screenshot NBC excel application]({{base}}/assets/printoutNBC.png "NBC Screenshot")

An output file for each model is consists of a list of every word occuring in the corpus. The format looks similar to the table below.
<table>
<tr>
<td>information</td>
<td>207</td>
<td>208</td>
<td>0.0041823337</td>
<td>2.3159703876</td>
</tr>
</table>

add example out put and close up shot of classification output.

 The workflow for the application is as follows:

 * **Setup a project**. Settting up a project creates folder structure that includes, 2 folders for the training copra used to model the conditional probabilities, 1 folder for the unclassified documents and 1 folder for the classified files used to calculate the models accuracy.

 * **Load Folders with training files**. Place files already classified, in my case articles I had already classified as relevant or irrelevant after the hard exculsion criteria had been applied. Place a selection of these classified files in a folder to validate the accuracy o the model.

 * **Validate Model**. Classify preclassified files to test the accuracy of the model.

 * **Classify Unclassified files**. If the models validity is adequate then continue on to classify the unknown documents.

I discuss actually building the classifier in the next post.
