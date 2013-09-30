Developing R packages
------------------

As a modern statistician, one of the most fundamental contributions you can make is to create and distribute
software that implements the methods you develop. I have gone so far as to say if you write a paper without
software, [that paper doesn't exist](http://simplystatistics.org/2013/01/23/statisticians-and-computer-scientists-if-there-is-no-code-there-is-no-paper/).

The purposes of this guide are:

* To explain why writing software is a critical component of being a statistician.
* To give you an introduction into the process/timing of creating an R package.
* To help you figure out how to distribute/publicize your software.
* To remind you that ["the perfect is the enemy of the very good"](http://en.wikipedia.org/wiki/Perfect_is_the_enemy_of_good).
* To try to make sure [Leek group](http://www.biostat.jhsph.edu/~jleek/) software has a consistent design.<sup>1</sup>


Data analyses
------------------

Stay tuned for the [Leek group](http://www.biostat.jhsph.edu/~jleek/) approach to building/sharing data analyses. 



Why develop an R package?
--------------------

Cause you know, you do what [your advisor](http://www.biostat.jhsph.edu/~jleek/) says and stuff.

But there are some real reasons to write software as a statistician that I think are critically important:

1. You probably got into statistics to have an impact. If you work with [Jeff](http://www.biostat.jhsph.edu/~jleek/) it was probably to have an impact 
on human health or statistics. Either way, one of the most direct and quantifiable ways to have an impact on the
world is to write software that other scientists, educators, and statisticians use. If you write a stats method paper
with no software the chance of impacting the world is dramatically reduced. 
2. Software is the new publication. I couldn't name one paper written by a graduate student (other than mine) in the
last 2-3 years. But I could tell you about tons of software packages written by students/postdocs (both mine and at
other places) that I use. It is the number one way to get your name out there in the statistics community. 
3. If you write a software package you need for yourself, you will save yourself tons of time in sourcing scripts,
and remembering where all your code/functions are. 

Most importantly might be that creating an R package is _building something_. It is something you can point to and
say, "I made that". Leaving aside all the tangible benefits to your career, the profession, etc. it is maybe the
most gratifying feeling you get when working on research. 


When to start writing an R package
---------------------

As soon as you have 2 functions. 

Why 2? After you have more than one function it starts to get easy to lose track of
what your functions do, it starts to be tempting to name your functions _foo_ or _tempfunction_ or some other such 
nonsense. You are also tempted to put all of the functions in one file and just source it. That was what I did
with my first project, which ended up being an epically comical set of about 3,000 lines of code in one R file. 
Ask [my advisor](http://www.genomine.org/) about it sometime, he probably is still laughing about it. 


What you need
--------------------

To start writing an R package you need:

* R - I mean, unless you are a wizard.
* Your functions, each written in a separate file<sup>2</sup>.
* At minimum the following packages installed: _devtools, roxygen2, knitr_.
* If you are going to use C code: _Rcpp_
* [Install git](https://help.github.com/articles/set-up-git)
* A [github account](https://github.com/signup/free). 
* To read [Hadley's intro](http://adv-r.had.co.nz/Package-basics.html)
* To read [Karl's Github tutorial](http://kbroman.github.io/github_tutorial/)

Naming your package
---------------------

The first step in creating your R package is to give it a name. Hadley [has some ideas](http://adv-r.had.co.nz/Package-basics.html)
about it. Here are our rules:

* Make it googleable - check by googling it.
* Make sure there is no [Bioconductor](http://www.bioconductor.org/)/[CRAN](http://cran.r-project.org/) package with the same name. 
* No underscores, dashes or any other special characters/numbers
* Make it all lower case - people hate having to figure out caps in names of packages.
* Make it memorable and if you want serious people to use it - but don't be too cute. 
* Make it as short as you possibly can while staying googleable.
* Never, under any circumstances, let Rafa or Hector name your package.<sup>3</sup> 


Versioning your package
---------------------

Almost all of our packages will eventually go on [Bioconductor](http://www.bioconductor.org/). So we are going to use the [versioning
scheme](http://www.bioconductor.org/developers/version-numbering/) that is compatible with that platform (with some helpful suggestions from [Kasper H.](http://www.biostat.jhsph.edu/~khansen/)).

The format of the version number will always be x.y.z. When you start any new package the version number should be
0.1.0. Every time you make any change public (e.g., push to [GitHub](https://github.com/)) you should increase z in the version number. If 
you are making local commits but not making them public to other people you don't need to increase z. You should 
stay in version 0.1.z basically up until you are ready to submit to [Bioconductor](http://www.bioconductor.org/) (or [CRAN](http://cran.r-project.org/)) for release. 

Before release you can increase y if you perform a major redesign of how the functions are organized or are used. You
should never increase x before release. 

The first time you submit the package to [Bioconductor](http://www.bioconductor.org/) you should submit it as version number 0.99.z. That way on the next 
release of [Bioconductor](http://www.bioconductor.org/) it will get bumped to 1.0.0. The next devel version will get bumped to 1.1.0 on [Bioconductor](http://www.bioconductor.org/).
Immediately after releasing, if you plan to keep working on the project, you should bump your version on [GitHub](https://github.com/) to 1.1.0.

Thereafter, again you should keep increasing z every time you make a public change. If you do a major reorganization
you should increase y. 


Creating your package
---------------------

Run this code from R to create your package. It will create a directory called _packagename_ and put some stuff in it 
(more on this stuff in a second). 

```S
## Load the libraries
library(devtools)
library(roxygen2)
library(knitr)

## Create the package directory
create("packagename")
```


Put your package on [GitHub](https://github.com/)
-----------------------

All packages that are developed by the [Leek group](http://www.biostat.jhsph.edu/~jleek/) will be hosted on [GitHub](https://github.com/). The accounts are free and
it makes it so much easier to share code/get other people to help you with your code. Here are the
steps to getting your package on [GitHub](https://github.com/):


1. Create a [new Github repository](https://help.github.com/articles/create-a-repo) with the same name (packagename)
2. In the _packagename_ directory on your local machine, run the commands: _git init_
3. Then run: _git remote add origin git@github.com:yourusername/packagename.git_
4. Create a file in the _packagename_ directory called README.md
5. Run the command: _git add ._
6. Run the command: _git commit -m 'initial commit'_
7. Run the command: _git push -u origin master_



The parts of an R package
--------------------

### R functions

The R functions you have written go in the R/ directory in the _packagename_ folder. Each of your R functions
should go in a separate file with a .R extension. We are going to use capital R for the extension of the files. 

Why? Don't ask questions. 

If you define a new class call the .R file _classname-class.R_. For example, if you are creating the leek class of objects
it would be called _leek-class.R_. If you are defining a new method for the class it should
be named _newclass-methodname-method.R_. For example, a plotting method for the leek class would go in a .R file
called _leek-plot-method.R_. 


### DESCRIPTION 

The DESCRIPTION file is a plain text file that gets generated with the _create_ command. 

* The package name should go after the colon on the first line. 
* The package title should be a one sentence description of what the package actually does. 
* The description should be a one paragraph description that builds on the title. It should give a user
some idea about what kind of data your software should be used on, what the inputs are and what the outputs are. 
* The version should be defined as described above. 
* The authors field may have a @R before the colon which should be deleted. The authors should be in the format _author name <authoremail@host.com>_ for example: 
_Jeff Leek <jleek@jhsph.edu>_ and should be comma separated. 
* A maintainer field should be added with maintainers listed as comma separated files. You are the maintainer of your
package when you create it. See the section below on after you leave the [Leek group](http://www.biostat.jhsph.edu/~jleek/) for more information. 
* The dependencies (other R packages your software uses/depends on) should be listed in a comma separated list after
the R version. One of the dependencies should be the _kntir_ package for the vignette. 
* The License is required to be open source. I like GPL-2 or GPL-3 or the creative commons licences like [CC-BY-SA](http://creativecommons.org/licenses/by-sa/2.0/)
[this](http://www.tldrlegal.com/) is a good website for learning more about software licenses. 
* You should add a line _VignetteBuilder: knitr_ 
* You should add a line _Suggests: knitr, BiocStyle_ 

Documentation
---------------------

This is how I feel about the relative importance of various components of statistical software development:

![documentation](https://raw.github.com/jtleek/rpackages/master/documentation.png)

Ideally your software is easy to understand and just works. But this isn't Apple and you don't have a legion
of test users to try it out for you. So more likely than not, at least the first several versions of your 
software will be at least a little hard to use. The first versions will also probably be slower than you would
like them to be. 

But if your software solves a real problem (it should!) and is well documented (it will be!) then people will
use it and you will have a positive impact on the world. 

Documentation has two main components. The first component is help files, which go into the man folder. The second 
component is vignettes which will go in a folder called inst/doc/ which you will have to create. I'll tackle 
each of these separately. 

### Help (man) files


These files document each of the functions/methods/classes you will expose to your users. The good news is that
you don't have to write these files yoursef. You will use the _roxygen2_ package to create the man files. To use
_roxygen2_ you will need to document your functions in the .R files with comments formated in a specific way. Right
before your functions you should have a set of comments that are denoted by the symbol _#'_. They are structured in 
the following way:

```S
#' A one sentence description of what your function does
#' 
#' A more detailed description of what the function is and how
#' it works. It may be a paragraph that should not be separated
#' by any spaces. 
#'
#' @param inputParameter2 A description of the input paramater parameterName1
#' @param inputParameter1 A description of the input parameter parameterName2
#'
#' @return output A description of the object the function outputs 
#'
#' @keywords keywords
#'
#' @export
#' 
#' @examples
#' R code here showing how your function works

myfunction <- function(...){}
```
You include the _@export_ command if you want the function to be exported (i.e. visable) to your end users. [Hadley](http://had.co.nz/) has a pretty comprehensive [guide](http://adv-r.had.co.nz/Documenting-functions.html) where you can 
learn a lot more about how _roxygen_ works. Your function follows immediately after the comments. 

When you have saved functions with _roxygen2_ style comments you can create the .Rd files (the man files themselves) 
by running:

```S
document("packagename")
```
on the package folder. The package folder must be in the current working directory where you are editing. 

Please read [Hadley](http://had.co.nz/)'s [guide](http://adv-r.had.co.nz/Documenting-functions.html) in its entirety to understand how to document packages and in particular, how _roxygen2_ 
deals with collation and namespaces. 


### Vignettes

Documentation in the help files is important and is the primary way that people will figure out your functions if they
get stuck. But it is equally (maybe more) critical that you help people _get started_. The way that you do that is to
create a vignette. For our purposes, a vignette is a tutorial that includes the following components:

* A short introduction that explains 
  * The type of data the package can be used on
  * The general purpose of the functions in the package
* One or more example analyses with
  * A small, real data set
  * An explanation of the key functions
  * An application of these functions to the data
  * A description of the ouput and how it can be used
  
We will write Vignettes in [knitr](http://yihui.name/knitr/). We will put a file called vignette.Rmd in
the directory packagename/inst/doc/. [Here](http://yihui.name/knitr/demo/vignette/) is some information
from [Yihui](http://yihui.name/) about building vignettes in knitr. You should use the [BiocStyle](http://www.bioconductor.org/packages/devel/bioc/html/BiocStyle.html)
package to style your vignette. This means you will need to add this code to the preamble of your markdown
file:

```S
<<style, eval=TRUE, echo=FALSE, results='asis'>>=
BiocStyle::latex()
@
```

See the [BiocStyle vignette](http://www.bioconductor.org/packages/devel/bioc/vignettes/BiocStyle/inst/doc/LatexStyle.pdf) for
commands that you can use when creating your vignette (e.g. _\Biocpkg{IRanges}_ for referencing a [Bioconductor](http://www.bioconductor.org/) package).



Who should be an author?
---------------------

For our purposes anyone who wrote a function exposed to users in the package (a function that has an @export in the documentation)
will be listed as an author. 

Who should be a maintainer
---------------------

You will be a maintainer and [Jeff](http://www.biostat.jhsph.edu/~jleek/) will be a maintainer. If you can sucker one of your fellow students into maintaining the pakcage
as well, you can list them, but they must make the same commitment to 5 years of support. 

[Good writers borrow from other authors, great authors steal outright](http://www.brainyquote.com/quotes/quotes/a/aaronsorki405048.html)<sup>4</sup>
---------------------

One thing I think a lot of academics (definitely including myself here) struggle with is the need to be "new" and 
"innovative" with everything they do. There is a strong [selective pressure](http://en.wikipedia.org/wiki/Evolutionary_pressure)
for these qualities in academia.

But when writing software it is very, very important not to reinvent every single wheel you see. One person who 
is awesome at blending existing tools and exponentially building value is [Ramnath Vaidyanathan](https://github.com/ramnathv).
He built [slidify](https://github.com/ramnathv/slidify) on top of knitr and [rCharts](https://github.com/ramnathv/rCharts) 
on top of existing D3 libraries. They allowed him to create awesome software without having to solve every single problem.

Before writing general purpose functions (say for regression or for calculating p-values or for making plots) make sure
you search for functions that already exist (or ask [Jeff](http://www.biostat.jhsph.edu/~jleek/)/your fellow students if you aren't sure).

An important and balancing principle is that you should try to keep the number of dependencies for your package as 
small as possible. You should also try to use packages that you trust will be maintained. Some ways to tell if a package
is "trustworthy" are to check the number of downloads/users (higher is better), check to see if the package is being
actively updated (on [GitHub](https://github.com/)/[Bioconductor](http://www.bioconductor.org/)/[CRAN](http://cran.r-project.org/)) and there is a history of updates, and check to see if the authors of the
packages routinely maintain important packages (like [Hadley](http://had.co.nz/), [Yihui](http://yihui.name/), [Ramnath](https://github.com/ramnathv), [Martin Morgan](http://www.fhcrc.org/en/util/directory.html?q=martin+morgan&short=true#peopleresults), etc.). Keep in mind your
commitment (see below) when making decisions about whose functions to use. 

Simple >>>> Complex
---------------------

A major temptation for everyone creating code is to generate a bunch of features they think that users will want without
actually testing those features out. The problem is that each new feature you create in your package will monotonically
increase the number of dependencies and the amount of code you have to maintain. In general, the principle should be
to create exactly enough functions that the users can install your package, perform your analysis, and return the results.

Specifically, be wary of things like GUIs or Shiny apps. Given the heavy emphasis placed on reproducibility these days,
it is rarely the case that real/important analyses will be performed in a point and click format. 

If you are way into creating products that point-and-click users will be interested in I'm very happy to support you in 
that, since I think those things are cool, probably the future, and can certainly raise the interest in your work. But
they present a potentially major difficulty in maintainence and should be placed in separate packages on your own repository. 


Unit tests
---------------------

For now, unit tests aren't a required component of packages in the [Leek group](http://www.biostat.jhsph.edu/~jleek/). On the other hand, they can be hugely useful,
especially as you maintain packages that you haven't worked on in a while. My suggestion is to use the [testthat](http://adv-r.had.co.nz/Testing.html)
framework to build tests before you leave (or [Bioconductor](http://www.bioconductor.org/)'s [unit testing](http://www.bioconductor.org/developers/unitTesting-guidelines/))
A couple of suggestions:

* Organize your tests into contexts (groups of tests) and name them.
* Have some tests that test the basic helper functions on fixed data.
* Have some tests that check the output of your main functions on fixed data. 


Dummy proofing
---------------------

Hopefully your package will have a ton of users. Inevitably, they will try to use the software for purposes you did not
intend. Some of these will be happy things (software you wrote for microarrays being used in sequencing). Sometimes
they will be unhappy - people using it completely out of context and getting wonky answers. 

You should do some minimal checking of arguments and have your functions "fail gracefully". But a major component
of dummy proofing is writing thorough documentation and vignettes (see the above sections). The major way we will
focus on dummy proofing is through documentation - and when you see weird cases, add them to the documents. 


Releasing to [Bioconductor](http://www.bioconductor.org/)
---------------------

Once you have developed a package you should use it yourself for a couple of weeks. Then you should 
have at least one other student use it without you giving them any instructions other than telling them
where some data are - that way you can test your documentation. Finally, you should meet with [Jeff](http://www.biostat.jhsph.edu/~jleek/) and 
get ready to release it to [Bioconductor](http://www.bioconductor.org/). 

The steps in releasing to [Bioconductor](http://www.bioconductor.org/) are:

1. Follow [Bioconductor](http://www.bioconductor.org/)'s [checklist](http://www.bioconductor.org/developers/package-submission/) to confirm
the package is ready to upload
2. Update the version number and push to [GitHub](https://github.com/). In the commit comments, state it is the version being 
pushed to [Bioconductor](http://www.bioconductor.org/).
3. Send an email as described in the checklist stating that you want an account and want to submit a package. 
4. Submit the package to [Bioconductor](http://www.bioconductor.org/).
5. Update the version number (bump y and z) to the next odd number for z. In the commit comments, state
that this is the new devel version. 


Your commitment
---------------------

You are vested in creating the software you are working on now since it is part of your training and will definitely
help your short term career in terms of getting jobs/gaining visibility. But your time as a graduate student is
(happily for you) limited. After a few years you will graduate and head off to an awesome job. The software you 
built as a student may be less important to you. 

Hopefully, though, it is still very important to the users who are applying that software to solve the most pressing
problems in molecular biology and medicine. It is unfair to those users if your software breaks down and can't be
installed/used. 

So if you undertake a research project in the [Leek group](http://www.biostat.jhsph.edu/~jleek/) which involves software development (pretty much all of them will)
you commit to maintaining that software for at least 5 years after you graduate. Of course, I can't hold you to that
like a contract or anything, but think of it as an honor thing. 

5 years is a long time. It is most of the way toward tenure (in academia) or probably 3 years after you have started your
own awesome company and appeared on Techcrunch. So it is worth thinking about ways you can ensure that the maintainence
will be as low as possible. Specifically:

* Make the dependencies as minimal as possible. If your dependencies update, you'll have to update the software
* Only create functions that are absolutely necessary for the package. It is hard to delete functions from a package
after it is released without messing with users and adds to the maintainence headache each time you keep one in. 
* Make the vignette really clear and add a FAQ with questions you get from users while you are still in the [Leek group](http://www.biostat.jhsph.edu/~jleek/). 
* Add comments to your code/unit tests so that when something breaks you can find/fix the problem quickly. 


About the author
--------------------

The first version of this tutorial was written by [Jeff Leek](http://www.biostat.jhsph.edu/~jleek/)  ([@simplystats](https://twitter.com/simplystats)). Hopefully
he can sucker his students into contributing since they are much, much better R programmers than he is. 

### Footnotes

1. These design requirements are subject to update and may not reflect [Leek group](http://www.biostat.jhsph.edu/~jleek/) software created before 9/18/2013 
(or ever really, remember the perfect is the enemy of the very good).
2. Except for utility functions, more on this later. 
3. Ask me about the time they named one of my packages "succs"
4. With proper attribution, of course.


