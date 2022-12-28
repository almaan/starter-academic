---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "What it takes to book a campsite in the US"
subtitle: "Why and how I wrote a bot to look for available campsites.."
summary: "Imagine that you really want to go camping in any of the more popular
National Parks here in the US, but there's fierce competition to get the spots, what do you do to get an edge? You obviously write a bot to beat the other people to it." 
authors: ["admin"]
tags: ["random",'outdoor','hikes','python']
categories: ["personal project"]
date: 2022-12-27T00:53:05+01:00
lastmod: 2022-12-27T10:53:05+01:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: ["camping"]
---

## TLDR:
I built a Python application that checks the availability of specific campsites
listed at [Recration.gov](https://www.recreation.gov), and if there is anything
available, it sends you an email. The code can be found
[here](https://github.com/almaan/campsite-checker).

## Intro

A few weeks ago, me and some friends were eating popcorn at a bar in Oakland
(genious concept); in addition the the usual banter, we ventured into the domain
of hiking and camping. Especially, how hard it is to get a spot at the campsites
in the more "popular" recreational areas (e.g., Yosemite). I am yet to figure
out exactly what laws that apply and where you can (legally) camp, but I'm not
stoked about getting shot for trespassing, so going to a designated campsite
seems like the safer option. However, there are some issues with this, which my
friends explained as follows:

* Almost everyone wants to use the campsites (because of the convenience)
* There's only a limited number of spots at each campsite
* Due to the high demand, spots are being booked almost instantly after being
  released

One of my friends also made the following statement:

> All the engineers in SF know how to code, so they just write bots that book
> all the campsites for them, meaning 'normal' people never have a chance to book anything.

Now, I'm not a software engineer and I very rarley write code to interact with
the web, but boldened by a few drinks I volunteered to assemble something that
would help us in the battle of these coveted campsites. So that's, in a
nutshell, how I decided to write a bot of our own.


## Project

<ins>Initial objective</ins>: **Create a bot that will scrape the web for available campsites
and notify the stakeholders if anything becomes/is available.**

After thinking a bit more about the intital objective and looking into the
resources, a more specific objective and workflow emerged:

* Instead of scraping 'the web' - the focus would be on a single site
  [Recration.gov](https://www.recreation.gov). This holds _a lot_ of campsites
  (113'000) and seemed to have a decent web API that you could query for available dates
  (though with limited options)

* Not every campground needs to be checked, only those of interest to us. This
  should be amenable to change, i.e., adding/removing campgrounds to our list
  needed to be seamless.

* We're not available all the time, so campground availability should only be
  reported within certain windows of time.

* Minimal effort should be required to take part of the information. Because...
  well I'm lazy and busy.

* The bot should not book anything, there are several reasons for this, but the
  two main ones are:

  * I don't want to get into payments and validation of credit cards etc., that
    feels a bit risky (especially if you need to host that on an external
    site)

  * I don't want to risk excessive booking or unwanted transactions in case any
    bugs enter the codebase

*  We want to assess the availability continously, so repeated queries should be made.
   However, we want to avoid redundant information - _"if you don't have anything
   new to say, then don't say anything at all."_

Having established what features I needed out and formalized _what_ I actually
wanted to create, it was just to start building. Also, I decided to write this
in Python, because that's the language I'm most comfortable with - and I wanted
this to be a small project not involving learning a new syntax/language.

The very first thing I had to do was to understand how to query Recration.gov
for availability. They have an [API](https://github.com/almaan/campsite-checker)
with documentation, but this was honestly not very helpful. Browsing 
other github projects with similar objectives. By doing so, I realized that I
could get all the information that I needed by sending a `GET` request to their
website. The tricky part was to understand exactly how this worked, after some
time, I managed to wrap my had around it. I could get what I wanted by using the `get` function from the
`requests` module to send the `GET` request, passing it the follwoing arguments:

* <ins>url</ins>:
    `https://www.recreation.gov/api/camps/availability/campground/CAMPGROUND_ID/month`
    Where `CAMPGROUND_ID` is the ID of the campground you are interested in. This ID
    is easily extracted from the campground's URL. For example, the campground
    "Rob Hill" has the following URL:
    `https://www.recreation.gov/camping/campgrounds/10172170`. The ID of Rob
    Hill is `10172170`
* <ins>headers</ins>: any valid header seemed to work, but I went with `{"User-Agent":"Defined"}`
* <ins>params</ins>: this was actually the trickiest part, you need to
  specify a "start date", but this **has** to be the first of the month you are
  interested in, and also the format is very specific, namely:
  `YYYY-MM-01T00:00:00.000Z`. A bit peculiar (to me at least) but easily done
  once I figured out the pattern.

A successful query would return status `200` and a response object with a
`.json` method, from which a dictionary containing all the relevant information
can be extracted.

A brief comment on camping lingo:
> a **campground** is a larger area that
> holds multiple **campsites**. At Recretion.gov you search for a campground and
> get availability for the different campsites within that campground.

The dictionary obtained from our (hopefully) successful query, will hold
different layers of information, but what's most interesting to us the data
accessed using the `campsites` key; this is a new dictionary with an entry for
each campsite within the queried campground; among an abundance of information,
the availability is listed for all
dates in the specified month. So this is exactly what we are looking for. The
only addditional implementations was a loop over all the months of interest and
filtering to only include dates where sites are available.

I'm using a `config` file (YAML) to specify the some of the parameters to this
application, such as the months of interest (one could do specific dates, but I
found it more conveinient to just say which month you want to check for dates
since we want as many options as possible). I found a nice package called
[_dateparser_](https://dateparser.readthedocs.io/en/latest/) to help with
parsing of the dates, using this allows for a more sloppy and more human
friendly entry of dates.

Finally, the availability information is converted to an HTML text output which
can be saved as either an html file or added as the body of an email. HTML was
the first 'programming' language I learned, unfortunately my skills peaked at
12y/o... so all I did was to convert this to a very basic table listing the
campsites for each campground together with the available dates. For each
_campsite_ there's also a direct link to the booking page. Something like:

```html
Campground: Point Reyes National Seashore Campground

    Campsite: BOAT A, 1-6 people
      2023-03-01
      2023-03-02
      2023-03-03
      2023-03-04
    Campsite: BOAT B, 7-14 people
      2023-03-01
      2023-03-02
      2023-03-03
      ...
```

Now, one of the previously listed criteria was that it should easy to add/remove campgrounds, and
what's more, I wanted different people to be able to make these changes -
several people are in on this, and I don't want every single change to go
through me (that'd be a massive bottleneck). The solution to this was to create
a _public_ Google Sheets document, where anyone we shared the link with could
add/remove campgrounds as we went along. This was also very easy to implement, I
used the `subprocess` module's `run` function to make a `curl` query, more
specifically:

```bash
curl -L https://docs.google.com/spreadsheets/d/SHEET_ID/export?format=csv&gid=GID --output TEMPFILE
```

Where `SHEET_ID` is the ID of your sheet id and `GID` is the tab id, you can
grab both of these from the URL of your sheet.
I found [this](https://stackoverflow.com/questions/24255472/download-export-public-google-spreadsheet-as-tsv-from-command-line)
stackoverflow post quite helpfull.

As for how updates should be shared with those of us who are in on this
"scheme", I settled on something that feels as old as stone tablets: email.
Everyone checks their email, we already have apps for it, and it's easy to set
up. I'll loop back to the email feature soon - but exactly how this would be
implemented was more or less completely dictated by my choice of hosting
service, so let's say something about how that first.

I decided to host this at
[pythonanywhere.com](https://www.pythonanywhere.com) which was affordable
and seemed to allow enough customization. They also had a "Tasks" feature where
you could launch reoccuring tasks just like a cronjob. Being cheap, I first went
for the free subscription plan - and this made me sign up for 
[mailgun.com](https://www.mailgun.com/) as a service for email. This site was
whitelisted for outgoing requests and didn't require
loads of configurations to be made (I'm looking at you Gmail...). Now, in the
end I went for a paid subscription of pythonanywhere.com, but I stayed with mailgun
anyway. Why? Well honestly, they had a really simple API, for real - to send an
email, all you need is your API key and the follwoing function:

```python
def send_simple_message():
    return requests.post(
        "https://api.mailgun.net/v3/YOUR_DOMAIN_NAME/messages",
        auth=("api", "YOUR_API_KEY"),
        data={"from": "Excited User <mailgun@YOUR_DOMAIN_NAME>",
              "to": ["bar@example.com", "YOU@YOUR_DOMAIN_NAME"],
              "subject": "Hello",
              "text": "Testing some Mailgun awesomness!"})
```

I'm not a complete idiot so I'm listing me as the receiver (to) of all emails while
the other people in the group are bcc:ed. We all know each other, but as this
grows it's nice to not expose everyone's email.

Now, what I didn't register immediately with mailgun.com was that you needed an
external domain to send your emails through (to avoid spamers... I think?).
Fortunately, I had *this* one (`differentiable.net`) to use. It required me to
setup some DNS configurations, but following their guide it took me at most
~20min.

To avoid redundant emails with the same availability information being sent over
and over again, I opted for a semi-lazy solution. I'm saving the *md5sum* of the
latest email (content) query, and if the result from a new query is the same -
no email is sent. Evidently, this means that _any_ change would prompt a new
email, additions of campsites as well as campsites being booked. The former is
probably the most interesting, but filtering for this would require a bit more
work (maybe for the future) and it's kinda good to know when spots begin to
disappear.

Ok, almost there. I wrapped the workflow into a CLI where the two input
arguments consit of the paths to each of two config files (YAML). Without
getting into the nitty gritty details, one of these config files (static)
contains emails to the people who should recieve the updates, API keys etc., this
information never really changes and I don't keep it in a public space. The
other config file (dynamic) contains paths to the output directory as well as
the dates that should be queried; this I keep at github, for reasons that soon
will become clear.

Finally, I wrote a bash script that runs the CLI application every hour (by
using the Task feature at pythonanywhere.com). However, in the bash script,
before running the 'campsite checker' (the incredibly creative name of this bot)
I pull the source code from its github repo. Hence, any updates/changes to the
code or the dynamic design file will be integrated in the next query.

So that's about it. You can find the code
[here](https://github.com/almaan/campsite-checker/), although with limited
documentation as of now.

In total I'd estimate that this project took somewhere between 5-8h of my time,
which isn't very much. It was a fun exercise in web interaction and made me
somewhat inclined to look into JavaScript and React. Maybe for the next project?
