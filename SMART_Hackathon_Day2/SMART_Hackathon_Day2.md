So day 2 of the #JHUSMARTHack was last week, but I figured this would be a good time to discuss what was accomplished.  I created some packages that are somewhat specialized and aren't fully finished yet, so I'll hold off.  What I really want to discuss though is why I like using [RStudio](http://www.rstudio.com/) for making packages. 

# What was accomplished?
I had spoken [before](http://hopstat.wordpress.com/2014/05/01/smart-hackathon-day-1/) about the [repositories](http://github.com/muschellij2/) I had worked on, and also about [Developing Packages in RStudio](http://www.rstudio.com/ide/docs/packages/overview).  I'll discuss the workflow I settled into for for making a package.

## Workflow for an `R` package
I'm assuming your `R` code is already available, presumably functions you had created during a project or analysis.  If code is not available, GREAT!  You can start your workflow for your new package or product all the same.  I'll try to put command-line equivalents in double brackets [[ ]].

Workflow:

1.  Start RStudio.
2.  Go to `File -> New Project`.  (Save any unsaved work).
3.  Select `New Directory`.  Now this may be counterintuitive if you have work saved, but if you're creating a package choose this.  This will setup a new folder and copy over any code you have already created.
4.  Select `R Package`.  This will allow you to name your package (it will be used in the `library` statement unless changed later); let's call it `mypckg`.  You can also choose code you have to operationalize, e.g. put into a package.  Also - select you want to create a git repository.  
5.  Voila! The folder is created where you have the components of an `R` package, such as the documentation (`man`), extra install dir (`inst`), the `R` code (`R`), etc.  [[`library(devtools); create("mypckg");`]]

Now, here is one of the main reasons I like using RStudio for projects: the `.Rproj` file.  The `.Rproj` file is an RStudio project file.  It allows you to work on stuff in multiple tabs/scripts, then close the project, and pop up the other tabs/scripts you were working on before opening up that project.  If you are in RStudio, the top right should show a `Project: None` if you don't have a project loaded.  These project files allows me to segregate my workflows and scripts, and they help me organize a bit more.  I highly recommend checking out [Hilary Parker's post](http://hilaryparker.com/2014/04/29/writing-an-r-package-from-scratch/) before continuing, especially if you're not an RStudio fan.

### Using RStudio Build Tools
Now, when I say RStudio Build Tools, I essentially mean wrappers for the [`devtools`](https://github.com/hadley/devtools) package.  The package is amazing (hardly shocking since Hadley Wickham is the main author), and along with the `roxygen2` package, allow package creation to be as easy as possible. 

Now, let's set up our options for Build Tools.  In RStudio, go to `Build -> Configure Build Tools` (again you must be in an RStudio project).  For `Check Package`, I recommend putting the `--as-cran` option (especially if you plan to [submit to CRAN](http://cran.r-project.org/web/packages/policies.html).  You should also see a checkbox saying `Generate documentation using Roxygen`. If this is not available, run `install.packages("roxygen2")`, close and reopen the project.  Check this box, and click the `Configure` button and I usually click all options.  


### Setting up a remote git repository
Before, we checked for a git repository to be created.  Now, you can create a new repository in your favorite GitHub remote repository.  Mine is [GitHub](http://www.github.com).  You can use the GUIs such as the GitHub GUI or SourceTree, but I generally set this up using the Terminal by just [adding the remote](https://help.github.com/articles/adding-a-remote).  ([Here](https://help.github.com/articles/generating-ssh-keys) is a link to create ssh keys so you don't have to type in passwords for git).  Now, if you restarted the RStudio project, go to `Build -> Configure Build Tools` and you should see the remote repository if you click the `Git/SVN` tab.  

Now that the repository is set up (even if you don't use a remote repository), you can go to `Tools --> Commit` to commit to the repository.  This allows you to add and stage the changed files while adding a commit comment.  You can also see a visual history of the differences and changes as well as do much of what you would need to from the command line.  Again, I like the Terminal, but I like having this all in one program and not having to switch back and forth.  

### DOCUMENTATION!  EXAMPLES!  VIGNETTES!
Now that you have everything set up, you have to do the big things that differentiate a bunch of functions from a package: documentation and examples (including vignettes).  Again, for documentation, we'll be using the `roxygen2` package.  Roxygen is essentially a format that starts with a line with `#' ` followed by `@` followed by a "tag".  The tags can be found at `?rd_roclet`.  Now, I highly recommend vignettes, but I'm not an expert on these and think we'll just stick to function docs right now.

#### Jumping to Sublime Text
Before we start documentation, let me again tell about MY workflow rather than Roxygen.  My workflow now jumps to [Sublime Text](http://www.sublimetext.com/3).  I have [`Line-by-line`](https://github.com/alyssafrazee/Line-by-line) installed (which you will need), which allows me to run a script to parse an `R` function and create the necessary Roxygen tags.  See [Alyssa's post](http://alyssafrazee.com/rdocs.html) for the description and a more command-line workflow for `R` packages.  

Now that we're in Sublime Text, I open the `.R` files from my packages `R` directory.  Select the function definition such as `x = function(z, y, l=4, ...){` and use `CMD+D` to create Roxygen tags!  This is like meta-programming for documentation: running scripts to make documentation (granted it's in Python).  As an aside, one powerful feature of this documentation is that if you have code as:
```
LFPCAg <- function(
Y,# an n x D matrix
                   # Y is assumed to be centered by its mean function
                   gridpoints = 1:ncol(Y),       # a vector of grid points along curves, corresponding to columns of Y 
                   Zlist=NA,
                   G=NA,
                   Ivec=NA,
                   ...){
```
this will parse the Roxygen tags as the comments for each argument/parameter (even if multi-line!):
```
#' @title <brief desc>
#'
#' @description <full description>
#' @param Y an n x D matrix Y is assumed to be centered by its mean function
#' @param gridpoints a vector of grid points along curves corresponding to columns of Y
#' @param Zlist
#' @param G
#' @param Ivec
#' @param ... 
#' @export
#' @keywords
#' @seealso
#' @return
#' @aliases
#' @examples \dontrun{
#'
#'}
```

This also puts in your mind, even if you're only creating functions and not a package, that you'll almost have documentation ready made when using this function format from day 1.

#### Jumping Back to RStudio
Now, opening these Roxygen-tagged functions, I can fill in the rest in RStudio.  One thing to note is that RStudio will assume you're trying to stay in Roxygen notation with a return of line (which is great for multi-line descriptions/titles/etc).  Also, if you have `#' @` starting a line, then RStudio will do tab completion of Roxygen tags.  Not leaps and bounds saved on time, but hey, I like tab completion.  

Now you have to write your examples, the description of arguments (denoted as parameters), the overall function description and title, and  use the `@export` to allow this function accessible to the user.  One note is that if you depend on another function or package, use the `@import pkgname` or `@importsFrom pkgname::funcName` tags.  `R CMD check` will warn you if you don't have anything in `@keywords`, `@aliases`, or `@examples`, so remove these if not necessary.

### Just let me check my functions!
If you're still working on the package and want to play with functions and no so much the documentation, you can use `Build -> Load All` [[`devtools::load_all`]] to load the functions (even those not exported) into memory.  

### Compile and Load
Now let's fast-forward to when you have created the the documentation for your functions.  While still in your project, go to `Build -> Build and Reload` to get your package loaded into memory [[`devtools::build` then `devtools::install`]].  Roxygen will create the docs. FYI - if you change around function names and recompile, the `man` folder may have obsolete `.Rd` files, so you can delete old ones.  

You should edit the `DESCRIPTION` file to change some specifications, such as `Depends:` fields for package dependencies. That's documented many places on the web to find about what goes in there.

Now edit your functions and docs, push to the remote repository and then allow people install your package by using:
```
library(devtools)
install_github("mypckg", "myGitHubUserName")
```
and there you have it - you've released software.  `Build -> Check Package` is good for testing your package (will tests your examples) and make sure everything looks OK.

# Conclusion
`R` package creation seems like a daunting task.  You can use tools in RStudio such as `Code -> Extract Function` to take loads of code to try to functional-ize it.  When you have a collection of functions, creating an RStudio project allows you to separate your package creation process from regular RStudio analysis and use, let's you have a one-stop shop for git version control, building, and checking of packages.  It let's you get over any hurdles of learning new functions in `devtools` (which may not be a good thing) and get you running in a short amount of time.  The Sublime Text plugin is not a crucial step, but can allow you to parse semi-documented functions and create a Roxygen header that's partially filled in.  This workflow allowed me to develop multiple projects and get them documented quickly at the hackathon.  

Hopefully this helps and good luck packaging!
