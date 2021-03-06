---
layout: post
title: Go Challenge - 2
tags: [go challenge, golang]
---

![update.jpg](/images/update.jpg)

* This second Go challenge is now closed.
* There were 105 entries from 30 countries.
* Evaluator Jeff R. Allen talks about your submitted code [here](http://blog.nella.org/golang-challenge-2-comments/) and [here](http://blog.nella.org/doing-it-the-hard-way/).
* Some of the participants blogged about their Go challenge experience: [Doug Cichon](http://www.codeforthought.net/approaching-golang-challenge-two) and [Jason Clark](http://jjasonclark.com/go_challenge_2_review_response/).
* The winners have been announced: #1 Michael Alexander (his [solution](https://github.com/golangchallenge/GCSolutions/tree/master/april15/normal/michael-alexander)), #2 David Le Corfec (his [solution](https://github.com/golangchallenge/GCSolutions/tree/master/april15/normal/david-le-corfec/challenge2)), #3 William Shallum (his [solution](https://github.com/golangchallenge/GCSolutions/tree/master/april15/normal/william-shallum)), #4 Luke Champine (his [solution](https://github.com/golangchallenge/GCSolutions/tree/master/april15/normal/luke-champine)) and #5 Arran Walker (his [solution](https://github.com/golangchallenge/GCSolutions/tree/master/april15/normal/arran-walker/gochallenge2)). According to the challenge author, David Le Corfec and William Shallum were close to one another.

#### The April 2015 Go Challenge for developers (newbies included)

#### Guillaume J. Charmes: Author of the second Go Challenge

<img align="right" src="/images/guillaume-j-charmes.jpg" height="200" width="200" alt="Guillaume J. Charmes" title="Guillaume J. Charmes" style="border:5px solid black" />
The second Go Challenge author is [Guillaume J. Charmes](https://twitter.com/charme_g). French first, software developer second. He loves to travel and used to live in France, China, Russia, Réunion, San Francisco, Chicago and Miami. He also likes to travel between technologies and languages: He jumped from C and ASM to PHP to fall into Python to then migrate to Go with some sides of Ruby. He is coming from a low-level ASM/C background which he uses to teach as well as for web security. 

He started using Go in 2009 and fell in love with its syntax and concurrency model. He joined dotCloud, Inc. where he was the only Go developer for a while. This is when they started Docker..., in Go. Arrived to Docker 1.0, he moved on to Citrix, Inc. under the sun of Florida where he helps the moving process from Ruby to Go.

Guillaume has this to say about the challenge:

> "Data security is a very important matter and this challenge is a good way to dive in with the `NaCl` package and leverage the power of Go interfaces to implement a clean solution over the network."

--- 

#### The Go Challenge 2

##### Can you help me secure my company’s data transmission?

Last week, our competitor released a feature that we were working on in secret. We suspect that they are spying on our network. Can you help us prevent our competitor from spying on our network?

#### To get started

Check out the two files **main.go** and **main_test.go** [here](https://gist.github.com/creack/333f89f6aec5b789c1a0). These files are the starting point for this challenge.

![update.jpg](/images/update.jpg)

**3rd April 2015**: Guillaume has made 2 changes to the test files:

* The first change is minor and concerns Window user with dual stack. He has changed the `net.Listen("tcp", ":0")` to `net.Listen("tcp", "127.0.0.1:0")`. This shouldn't change anything for most people.
* The second is more problematic and would impact people using `io.Copy`. In TestReadWritePing, he has moved the `w.Close()` in the writing goroutine just like in the other tests.

#### Goal of the challenge

In order to prevent our competitor from spying on our network, we are going to write a small system that leverages [NaCl](http://nacl.cr.yp.to/) to establish secure communication. **NaCl** is a crypto system that uses a public key for encryption and a private key for decryption.

Your goal is to implement the functions in **main.go** and make it pass the provided tests in **main_test.go**.

#### Steps involved

The first step is going to be able to generate the public and private keys. Next, we want to create an `io.Writer` and `io.Reader` that will allow us to automatically encrypt/decrypt our data.

##### Part 1

Implement the following helpers that will return our NACL Reader / Writer.

```
func NewSecureReader(r io.Reader, priv, pub *[32]byte) io.Reader
func NewSecureWriter(w io.Writer, priv, pub *[32]byte) io.Writer
```

##### Part 2

Now that we can encrypt/decrypt message locally, it would be interesting to do so over the network!

We are going to write a server that will exchange keys with the client in order to establish a secure communication.

In order to be able to encrypt/decrypt, we need to perform a key exchange upon connection.

For the sake of the exercise, performing the key exchange in plain text is acceptable. (In a production system, it would not be acceptable due to MITM risk!)

Unfortunately, everybody has already left for the day, so let's write a secure echo server so we can test!

In order to test our echo server, we can do:

```
$> ./challenge2 -l 8080&
$> ./challenge2 8080 “hello world”
hello world
```

#### Requirements of the challenge

* Use the latest version of Go i.e. version 1.4.2
* Use only standard library and package(s) under `golang.org/x/crypto/nacl`.

#### Hints

* We consider that our messages will always be smaller than 32KB
* Most of the elements involved for encryption/decryption have fixed length
* `encoding/binary` can be useful for the variable parts
 
#### Further exploration

If you have liked this challenge, you can keep programming **outside** the main challenge. You can:

* create an actual user interface. It would be nice if both side had a prompt and could send more than one message at a time.
* Handle multiple client and group chat
* Add compression

Please keep in mind that cryptography is a complex subject matter, with many very subtle risks and mistakes to make. While it makes for a fun exercise, inventing your own crypto systems for production use is usually not a good idea and should be left to a handful of very talented people.

---

#### Challenge Rules and how to enter the Go Challenge?

By participating in this challenge, you agree to be bound by the Challenge Rules below:

* The Challenge is open to individuals.
* Evaluators cannot enter the challenge except under the "Just for Fun" category.
* Each entrant shall indemnify, defend, and hold JoshSoftware Pvt. Ltd. (who has sponsored the domain and is the organizer of these challenges) harmless from any third party claims arising from or related to that entrant's participation in the Challenge. In no event shall JoshSoftware Pvt. Ltd. be liable to an entrant for acts or omissions arising out of or related to the Challenge or that entrant's participation in the Challenge.
* Odds of winning depend on the number and quality of entries received. 
* All taxes, including income taxes, are the sole responsibility of the winners. 
* No prize substitution is permitted. 
* Create a zip of your Go source code and send the zip file to **golangchallenge [at] gmail.com before 18th of April 2015 (6 am IST). Use [this link](http://www.worldtimeserver.com/convert_time_in_IN.aspx?y=2015&mo=4&d=18&h=6&mn=0) to find the equivalent time in your city/country**. No new solutions will be accepted after that. In the email mention **your full name, country of residence, twitter or GitHub id (if any) and participating under which category - Just participating | Participating and exploring further | Just for Fun | Anonymous entry**. We are accepting anonymous submissions and will evaluate them too but then these participants are not eligible for the prizes. 
* We will give your zip file to the evaluation team. 
* We shall be publishing on this blog, a list of participant names. If you don't want your name to appear kindly mention the same in your email. 
* You are allowed to re-submit your code if you feel it is necessary.
* **Note**: Avoid sharing your code with anyone else; if your solution becomes available to the general public it might impact evaluation of your submission.
* After the challenge is over, all submissions will be made available [online on GitHub](https://github.com/golangchallenge/GCSolutions) under the [BSD 3-Clause License](http://opensource.org/licenses/BSD-3-Clause) or the [GNU General Public License, version 3 - GPL-3.0](http://opensource.org/licenses/GPL-3.0) unless a participant has indicated that his/her solution should not be made public before the challenge ends.

#### How will the challenge be evaluated?

Entries will be anonymized and evaluated by the challenge author and a team of evaluators.

* Functioning code and a test suite that passes.
* Code hygiene. Use [gofmt](https://golang.org/cmd/gofmt/), [vet](https://godoc.org/golang.org/x/tools/cmd/vet) and [lint](https://github.com/golang/lint). Review [CodeReviewComments](https://code.google.com/p/go-wiki/wiki/CodeReviewComments).
* Readability. How easy is it for another programmer to grasp what your entry is doing?
* Code structure. Do types and files have good names?
* Reliability. Are errors properly handled?
* Appropriate consideration given to memory and performance (nothing is unnecessarily expensive).

#### Questions?

If you have any questions about this challenge, please join the [golang-challenge channel on slack](http://t.co/n6EesY9Mmv) and ask your questions. This is a room for people who are going to participate in the Go Challenge. You can also send us an email at golangchallenge [at] gmail.com

#### Evaluators

[Nathan Youngman](https://twitter.com/nathany) had set the guidelines for evaluation for the first Go Challenge. Subsequently [Dominik Honnef](https://twitter.com/dominikhonnef) modified the guidelines based on his experience as an evaluator for the first challenge. [Akshay Deo](https://twitter.com/akshay_deo), [Austin Riendeau](https://github.com/apriendeau), [Cory LaNou](https://twitter.com/corylanou), [Gautam Dey](https://github.com/gdey), [Jeff R. Allen](http://blog.nella.org/category/golang/), [Jyotiska NK](https://twitter.com/jyotiska_nk), [Kevin Gillette](https://twitter.com/kevingillette) and [Pravin Mishra](https://twitter.com/pravinmishra88) have agreed to go through all the submitted solutions of a challenge. They will comment and rank these solutions.

#### Best Solution

The author of the Go Challenge will decide the best five solutions. The author shall have the sole authority and discretion to select the award recipients. 

#### Notification

The winning entries will be announced here on this blog. The winners will be sent their prizes by email/postal mail.

#### Prizes

The author of the challenge will select 5 winners.

Here are some great prizes provided by our sponsors for the event.

_Winner 1_:

* [Anand D N](https://twitter.com/Wanderer140) - [Essential-Go screencast](https://www.kajabinext.com/marketplace/courses/1-essential-go)
* [Apcera](https://www.apcera.com/) - [Go Gopher Messenger Bag](https://www.googlemerchandisestore.com/shop.axd/Search?keywords=gopher)
* [CoreOS](https://coreos.com/) - Company T-Shirt and stickers and a free ticket to the [CoreOS Fest](https://coreos.com/fest/) in SFO.
* [Crowd Interactive](http://www.crowdint.com/) - A US$ 25 Amazon digital gift card.
* [Cube Root Software](http://cuberoot.in/) - An eBook [Go-The Standard Library](https://leanpub.com/go-thestdlib)
* [DigitalOcean](https://www.digitalocean.com/) - US$ 50 Amex Gift Card
* [GitHub](https://github.com/) - Free 1 year of a [Personal Micro Plan](https://github.com/pricing) and a [T-Shirt](http://github.myshopify.com/) of your choice.
* [Helpshift](http://www.helpshift.com/) - An eBook [A Go Developer's Notebook](https://leanpub.com/GoNotebook)
* [InfluxDB](http://influxdb.com/) - A US$ 50 Amazon digital gift card.
* [John Sonmez](https://twitter.com/jsonmez) - An eBook [Soft Skills: The software developer's life manual](http://www.amazon.com/gp/product/1617292397/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=1617292397&linkCode=as2&tag=satishtalimsw-20&linkId=WGSAUMHIF2SVWJ7D)
* [Koding](https://koding.com/Pricing) - A free Hobbyist plan for 3 months
* [Manning Publications Co.](http://manning.com/) - Two eBooks [Go Web Programming](http://www.manning.com/chang/) and [Go in Action](http://www.manning.com/ketelsen/)
* [NodePrime](http://www.nodeprime.com/) - An eBook [The Go Programming Language Phrasebook (Developer's Library)](http://goo.gl/mTwIOS)
* [O'Reilly](http://www.oreilly.com/) - An eBook [Mastering Go Web Services](http://shop.oreilly.com/product/9781783981304.do)
* [Packt Publishing](https://www.packtpub.com/) - A print book [Mastering Concurrency in Go](https://www.packtpub.com/application-development/mastering-concurrency-go)
* [Qwinix Technologies](http://www.qwinixtech.com/) - An eBook [The Docker Book: Containerization is the new virtualization](http://goo.gl/6sJJTy)
* [RainingClouds](http://rainingclouds.com/#!/) - An eBook [Level Up Your Web Apps With Go](http://shop.oreilly.com/product/9780992461294.do)
* [SoStronk](https://www.sostronk.com/) - [Counter-Strike: Global Offensive](http://store.steampowered.com/app/730/) game. You will need to have a free [Steam account](https://store.steampowered.com/join/?) to receive this gift.
* [Sourcegraph](https://sourcegraph.com/) - [Interview with the winner on their blog and will get their shirt, stickers](https://sourcegraph.com/blog)

_Winner 2_:

* [Anand D N](https://twitter.com/Wanderer140) - [Essential-Go screencast](https://www.kajabinext.com/marketplace/courses/1-essential-go)
* [Apcera](https://www.apcera.com/) - [Go Gopher Squishable](https://www.googlemerchandisestore.com/shop.axd/Search?keywords=gopher)
* [CoreOS](https://coreos.com/) - Company T-Shirt and stickers and a free ticket to the [CoreOS Fest](https://coreos.com/fest/) in SFO.
* [Crowd Interactive](http://www.crowdint.com/) - A US$ 25 Amazon digital gift card.
* [DigitalOcean](https://www.digitalocean.com/) - US$ 50 Amex Gift Card
* [Docker](https://www.docker.com/) - A ticket to [DockerCon 2015](http://www.dockercon.com/) in SFO, USA OR to DockerCon Europe (to be announced but probably in Dec. 2015).
* [GitHub](https://github.com/) - Free 1 year of a [Personal Micro Plan](https://github.com/pricing) and a [T-Shirt](http://github.myshopify.com/) of your choice.
* [Helpshift](http://www.helpshift.com/) - An eBook [A Go Developer's Notebook](https://leanpub.com/GoNotebook)
* [InfluxDB](http://influxdb.com/) - A US$ 50 Amazon digital gift card.
* [John Sonmez](https://twitter.com/jsonmez) - An eBook [Soft Skills: The software developer's life manual](http://www.amazon.com/gp/product/1617292397/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=1617292397&linkCode=as2&tag=satishtalimsw-20&linkId=WGSAUMHIF2SVWJ7D)
* [Koding](https://koding.com/Pricing) - A free Hobbyist plan for 3 months
* [Manning Publications Co.](http://manning.com/) - One eBook [Go Web Programming](http://www.manning.com/chang/)
* [NodePrime](http://www.nodeprime.com/) - An eBook [The Go Programming Language Phrasebook (Developer's Library)](http://goo.gl/mTwIOS)
* [Packt Publishing](https://www.packtpub.com/) - An eBook [Building Your First Application with Go-Video](https://www.packtpub.com/application-development/building-your-first-application-go-video)
* [Qwinix Technologies](http://www.qwinixtech.com/) - An eBook [The Docker Book: Containerization is the new virtualization](http://goo.gl/6sJJTy)
* [RainingClouds](http://rainingclouds.com/#!/) - An eBook [Level Up Your Web Apps With Go](http://shop.oreilly.com/product/9780992461294.do)
* [SoStronk](https://www.sostronk.com/) - [Counter-Strike: Global Offensive](http://store.steampowered.com/app/730/) game. You will need to have a free [Steam account](https://store.steampowered.com/join/?) to receive this gift.
* [Sourcegraph](https://sourcegraph.com/) - [Interview with the winner on their blog and will get their shirt, stickers](https://sourcegraph.com/blog)

_Winner 3_:

* [Apcera](https://www.apcera.com/) - [Go Gopher Squishable](https://www.googlemerchandisestore.com/shop.axd/Search?keywords=gopher)
* [DigitalOcean](https://www.digitalocean.com/) - US$ 50 Amex Gift Card
* [GitHub](https://github.com/) - Free 1 year of a [Personal Micro Plan](https://github.com/pricing) and a [T-Shirt](http://github.myshopify.com/) of your choice.
* [Manning Publications Co.](http://manning.com/) - One eBook [Go in Action](http://www.manning.com/ketelsen/)
* [NodePrime](http://www.nodeprime.com/) - An eBook [The Go Programming Language Phrasebook (Developer's Library)](http://goo.gl/mTwIOS)
* [Packt Publishing](https://www.packtpub.com/) - An eBook [Mastering Concurrency in Go](https://www.packtpub.com/application-development/mastering-concurrency-go)
* [Qwinix Technologies](http://www.qwinixtech.com/) - An eBook [The Docker Book: Containerization is the new virtualization](http://goo.gl/6sJJTy)
* [RainingClouds](http://rainingclouds.com/#!/) - An eBook [Level Up Your Web Apps With Go](http://shop.oreilly.com/product/9780992461294.do)
* [SoStronk](https://www.sostronk.com/) - [Counter-Strike: Global Offensive](http://store.steampowered.com/app/730/) game. You will need to have a free [Steam account](https://store.steampowered.com/join/?) to receive this gift.

_Winner 4_:

* [Apcera](https://www.apcera.com/) - [Go Gopher Squishable](https://www.googlemerchandisestore.com/shop.axd/Search?keywords=gopher)
* [DigitalOcean](https://www.digitalocean.com/) - US$ 50 Amex Gift Card
* [GitHub](https://github.com/) - Free 1 year of a [Personal Micro Plan](https://github.com/pricing) and a [T-Shirt](http://github.myshopify.com/) of your choice.
* [Manning Publications Co.](http://manning.com/) - One eBook [Go in Practice](http://www.manning.com/butcher/)
* [NodePrime](http://www.nodeprime.com/) - An eBook [The Go Programming Language Phrasebook (Developer's Library)](http://goo.gl/mTwIOS)
* [Packt Publishing](https://www.packtpub.com/) - An eBook [Mastering Concurrency in Go](https://www.packtpub.com/application-development/mastering-concurrency-go)
* [Qwinix Technologies](http://www.qwinixtech.com/) - An eBook [The Docker Book: Containerization is the new virtualization](http://goo.gl/6sJJTy)
* [RainingClouds](http://rainingclouds.com/#!/) - An eBook [Level Up Your Web Apps With Go](http://shop.oreilly.com/product/9780992461294.do)
* [SoStronk](https://www.sostronk.com/) - [Counter-Strike: Global Offensive](http://store.steampowered.com/app/730/) game. You will need to have a free [Steam account](https://store.steampowered.com/join/?) to receive this gift.

_Winner 5_:

* [Apcera](https://www.apcera.com/) - [Go Gopher Squishable](https://www.googlemerchandisestore.com/shop.axd/Search?keywords=gopher)
* [DigitalOcean](https://www.digitalocean.com/) - US$ 50 Amex Gift Card
* [GitHub](https://github.com/) - Free 1 year of a [Personal Micro Plan](https://github.com/pricing) and a [T-Shirt](http://github.myshopify.com/) of your choice.
* [Manning Publications Co.](http://manning.com/) - One eBook [Go in Practice](http://www.manning.com/butcher/)
* [NodePrime](http://www.nodeprime.com/) - An eBook [The Go Programming Language Phrasebook (Developer's Library)](http://goo.gl/mTwIOS)
* [Packt Publishing](https://www.packtpub.com/) - An eBook [Mastering Concurrency in Go](https://www.packtpub.com/application-development/mastering-concurrency-go)
* [Qwinix Technologies](http://www.qwinixtech.com/) - An eBook [The Docker Book: Containerization is the new virtualization](http://goo.gl/6sJJTy)
* [RainingClouds](http://rainingclouds.com/#!/) - An eBook [Level Up Your Web Apps With Go](http://shop.oreilly.com/product/9780992461294.do)
* [SoStronk](https://www.sostronk.com/) - [Counter-Strike: Global Offensive](http://store.steampowered.com/app/730/) game. You will need to have a free [Steam account](https://store.steampowered.com/join/?) to receive this gift.

Anyone can a get 42% off on the price of the following eBooks from [Manning Publications Co.](http://manning.com/):

* [Go Web Programming](http://www.manning.com/chang/) - Use discount code: **cftw15go**
* [Go in Action](http://www.manning.com/ketelsen/) - Use discount code: **cftw15go**
* [Go in Practice](http://www.manning.com/butcher/) - Use discount code: **cftw15go**

#### Winner Interviews

After a winner wins the monthly challenge, he/she would be interviewed by [Sourcegraph](https://sourcegraph.com/) and the interview will be published on their [blog](https://sourcegraph.com/blog).

#### The Participants

You can check out the list of participants for this challenge i.e. for [April 2015](http://golang-challenge.com/participants/april15/).

#### Challenge Solutions

All the solutions submitted by the participants are available **[here](https://github.com/golangchallenge/GCSolutions)**.

#### The Winners

![winner.png](/images/winner.png)

Guillaume J. Charmes has selected the winners of this challenge. They are:

<img align="right" src="/images/michael_alexander.jpg" alt="Michael Alexander" title="Michael Alexander" style="border:5px solid black" />
**Winner #1 Michael Alexander** is from Canberra, Australia, and currently a senior developer at [HRMWeb Pty Ltd](http://www.hrmweb.com.au/) working on [easyEMPLOYER](http://www.easyemployer.com.au/).

He was introduced to BASIC at an early age and has been writing code ever since. Always keen to try new languages and technologies, he dabbled in Go when 1.0 was released in 2012 and it stuck unlike many others did for him.

His [solution](https://github.com/golangchallenge/GCSolutions/tree/master/april15/normal/michael-alexander) to the challenge.

--- 

<img align="right" src="/images/david.jpg" alt="David Le Corfec" title="David Le Corfec" style="border:5px solid black" />

**Winner #2 - David Le Corfec** started programming on Amiga in the 90's. After Computer Science studies, he started in software development before changing to system engineering, supporting the web operations of the largest French broadcaster.

In 2013, he became interested in Go by stumbling upon groupcache, while researching caching solutions for streaming video. Shortly afterwards, Go replaced C as his favorite language, leading him to even consider a comeback to full-time programming!

His [solution](https://github.com/golangchallenge/GCSolutions/tree/master/april15/normal/david-le-corfec/challenge2).

---

Winner 3 - William Shallum (his [solution](https://github.com/golangchallenge/GCSolutions/tree/master/april15/normal/william-shallum))

Winner 4 - Luke Champine (his [solution](https://github.com/golangchallenge/GCSolutions/tree/master/april15/normal/luke-champine)) 

Winner 5 - Arran Walker (his [solution](https://github.com/golangchallenge/GCSolutions/tree/master/april15/normal/arran-walker/gochallenge2)). 

According to the challenge author, David Le Corfec and William Shallum were close to one another.

#### Sponsors

This challenge's sponsors are: [Anand D N](https://twitter.com/Wanderer140), [Apcera](https://www.apcera.com/), [CoreOS](https://coreos.com/), [Crowd Interactive](http://www.crowdint.com/), [Cube Root Software](http://cuberoot.in/), [DigitalOcean](https://www.digitalocean.com/), [Docker](https://www.docker.com/), [GitHub](https://github.com/), [GopherCasts](https://gophercasts.io/), [Helpshift](http://www.helpshift.com/), [InfluxDB](http://influxdb.com/), [John Sonmez](https://twitter.com/jsonmez), [JoshSoftware Pvt. Ltd.](http://www.joshsoftware.com/), [Manning Publications Co.](http://manning.com/), [NodePrime](http://www.nodeprime.com/), [O'Reilly](http://www.oreilly.com/), [Packt Publishing](https://www.packtpub.com/), [Qwinix Technologies](http://www.qwinixtech.com/), [RainingClouds](http://rainingclouds.com/#!/), [SoStronk](https://www.sostronk.com/) and [Sourcegraph](https://sourcegraph.com/).

#### Credit

* The Gopher character is based on the Go mascot designed by Renée French and copyrighted under the Creative Commons Attribution 3.0 license.
* [GitHub](https://github.com/) for the yearly sponsorship of a [GitHub Bronze Organisation plan](https://github.com/pricing) for the Go Challenge.
* The Go Challenge is being organized by [JoshSoftware Pvt. Ltd.](http://www.joshsoftware.com/) with help from the Go community.
