---
layout: post
comments: true
permalink: /:title.html
title: 'The &#39;Hull City Problem&#39; in Scala'
author: Richard
date: 2009/01/14
alias: /the-hull-city-problem-in-scala
---

<img src="/img/posts/flkexport2018/15989274720_4155a0af72_o.jpg" width="471" height="423" alt="4ff9dba8e1e8a-11219736-0-media_httpfarm4static_CDdAA">

Now that I have a physical copy of [Programming in Scala][] next to me,
the final excuse for me not spending more time with the language has
gone. In the interest of dipping my toe in the language (well, it's more
like my whole leg by now) I'm making a conscious effort to do all and
any scripty things I have to do in Scala.

One such script-like thing is the 'Hull City problem'. It's actually a
very simple problem, and now I think about it, it's more an assertion
than a problem: [Hull City][] [football][] club is the only club name in
the [English league][] that cannot be coloured in.

By which I mean, if you look at a name like "Liverpool", it is made up
of letters that are fully enclosed. If you were doodling you could
colour in the "o"s, the "p"s and the top of the "e". You can't do that
with "Hull City". Not in title case, not in upper case and not in lower
case. There's no other club in the league that you can say that about.

Or can you?

Clubs come and go, and even change name, so it'd be handy to have a
script that could test that the assertion is still true. Here's how I
went about it in Scala, once I'd decided it was partly a regular
expression problem. Yes, regular expressions are my [hammer][] of
choice.

Although not really necessary for this problem, I defined a class for
the name of the club and the level in the league (e.g., 1 is the Premier
League; 12 is Gloucester Northern Senior League), and a method for
deciding if the name can be coloured in:

    class Club(val name:String, val level:Int) {
      val failChars = "&ABDOPQRabdegopq"
      // We're given team names in Title Case, so that's what we check here.
      // The name can be coloured in if there exists a character in the name
      // that is contained in the list of "fail characters".
      def canBeColouredIn = name.exists(c => failChars.contains(c))
    }


That seems OK:

    $ scalac Club.scala
    $ scala Welcome to Scala version 2.7.2.final [...]
    Type in expressions to have them evaluated. Type :help for more information.

    scala> val hull = new Club("Hull City", 1)
    hull: Club = Club@bc9065

    scala> hull.canBeColouredIn
    res0: Boolean = false

    scala> new Club("Liverpool", 1).canBeColouredIn
    res1: Boolean = true `

To try this on all clubs you need to [export the list of teams from
Wikipedia][], which gives you an XML file that contains a list of club
names in a Wiki markup text format. A line in that file looks like this:
`|[[Aveley F.C.|Aveley]]||[[Isthmian League]] [[Isthmian League Division One North|Division One North]] (Level 8)`

And you also need some code to read the file and run a regular
expression over it:

    import scala.io.Source
    object Club  {

      def load(filename: String): Iterator[Club] =  {

        // Helper to remove the crud from team names:
        def clean(input:String) = input.replaceAll("&","&").
         replaceAll("AFC","").
         replaceAll("A.F.C.","").
         replaceAll("F.C.","").trim

        val fileContents = Source.fromFile(filename, "UTF8").mkString

        // Regular expression for extracting team name and level.
        val clubInfo = """\|\[\[([^|\]]+).*\(Level (\d+)\)""".r
        for(clubInfo(name,level) <- clubInfo findAllIn fileContents)
          yield new Club(clean(name),level.toInt)

      }

    }


A couple of points here: reading a file into memory using `Source` and
`mkString` is rather handy compared to what I'd usually do (i.e, using a
buffer and a loop, or grabbing a library that has it defined already);
creating a regular expression means calling `.r()` on a String which is
almost as short as having regular expressions as part of the syntax of
the language. But I think it's the `for` loop that stands out. The
`findAllIn` call looks straight-forward enough, but the left hand side
of that is cute. A regular expression in Scala, such as `clubInfo`, is
also an "extractor" in the language, which for me is probably best
explained by example as I don't have a full grip on all the concepts
yet:

    $ scala Welcome to Scala version 2.7.2.final [...]
    Type in expressions to have them evaluated. Type :help for more information.

    scala> val clubInfo = """\|\[\[([^|\]]+).*\(Level (\d+)\)""".r
    clubInfo: scala.util.matching.Regex = \|\[\[([^|\]]+).*\(Level (\d+)\)

    scala> val clubInfo(name,level) = "|[[Aveley F.C.|Aveley]]||[[Isthmian League]] [[Isthmian League Division One North|Division One North]] (Level 8)"
    name: String = Aveley F.C. level: String = 8

    scala> println(name)
    Aveley F.C.

    scala> println(level)
    8

Handy? I think so. Especially when you can use it in a for loop to split
out the matches into the two groups.

Putting it all together we can print out all the clubs that cannot be coloured in:

    $ scalac Club.scala
    $ scala Welcome to Scala version 2.7.2.final [...]
    Type in expressions to have them evaluated. Type :help for more information.

    scala> val clubs = Club.load("footballclubs.xml")
    clubs: Iterator[Club] = non-empty iterator

    scala> for(club <- clubs; if !club.canBeColouredIn)
    | println(club.name)
    Hull City

So there we have it. It's true: only Hull City can not be coloured in.
For now...


  [Programming in Scala]: http://www.artima.com/shop/programming_in_scala
  [Hull City]: http://en.wikipedia.org/wiki/Hull_City_A.F.C.
  [football]: http://en.wikipedia.org/wiki/Association_football
  [English league]: http://en.wikipedia.org/wiki/List_of_football_clubs_in_England
  [hammer]: http://en.wikipedia.org/wiki/Golden_hammer
  [export the list of teams from Wikipedia]: http://en.wikipedia.org/w/index.php?title=Special:Export&pages=List+of+football+clubs+in+England
