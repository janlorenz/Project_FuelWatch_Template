# How to do a Homework Project with your own Repository


- [Needed infrastructure](#needed-infrastructure)
- [Your own GitHub Repository](#your-own-github-repository)
- [Have git and GitHub working
  together](#have-git-and-github-working-together)
- [Clone your repository and move into the
  workflow](#clone-your-repository-and-move-into-the-workflow)

This is a generic explanation how to do a Homework Project in Data
Science Concepts and Data Science Tools courses.

## Needed infrastructure

You need a computer with Windows, MacOS, or Linux (maybe also other
exotic operating systems). A tablet, phone or Chromebook is probably not
sufficient.

Needed software:

- *Statistical Programming Language:* The instructions here are for
  [R](https://cran.r-project.org/), you can also use python but then you
  have to adapt instructions and setup yourself.
- *Version Control System:* [Git](https://git-scm.com/)
- *Integrated Development Environments* (IDE): The instructor uses
  [Positron](https://Positron.posit.co/). All instructions here are
  based this. In the past the instructor used RStudio. In principle you
  can use whatever you want.

Test that everything works together:

- When [R](https://cran.r-project.org/), [Git](https://git-scm.com/),
  [Positron](https://Positron.posit.co/) are installed, open Positron
  and check:
  - **Is R running?** On the top right you should see the R-symbol and
    its version numbers. That should be your current session. If not
    click on it, maybe it lists a python session or *Start Session*.
    Then a *Select Interpreter Session* dialog should pop up and you can
    start a new one and find R. If R is not offered solve this first by
    finding out how Positron can find the R installation. Usually it is
    detected automatically. Another check: Go the CONSOLE tab (usually
    at the bottom). Scroll to the start of the Console. It should start
    with something like `R 4.5.1 started` (if you have a higher version
    number, fine, probably). At the end of the console you should see a
    `>`-prompt. Type `version` and hit ENTER. You should see the version
    of R printed.
  - In R, you will use packages in the **package family**
    [**`tidyverse`**](https://www.tidyverse.org/). You can install it by
    running `install.packages("tidyverse")` in the CONSOLE.
  - **Do `git` and `quarto` run from the TERMINAL?** Go to the TERMINAL
    tab besides the CONSOLE. Type `git --version` and hit ENTER. You
    should see the version of git printed. Then type `quarto --version`
    and hit ENTER. You should see the version of quarto printed.  
    Note: You can also write `R` and hit ENTER. You start an R session
    in the Terminal. We do not need it, so type `q()` and ENTER to leave
    R again. (No need to save the workspace, just hit ENTER again if
    asked.)

**Learning:** Understand the difference between the CONSOLE and the
TERMINAL! Both are *command line interfaces* (CLI). The CONSOLE is for R
commands in the current session in Positron. The TERMINAL is for system
commands. It also works outside of Positron. We use it mainly for the
command line tools `git` and `quarto`. For both we will also use short
cuts and nice click and point interfaces in Positron. But it is
important to understand what happens in the background and that you can
always use the TERMINAL directly for all the things of git and quarto.

## Your own GitHub Repository

**Do you read this file in your own GitHub Repository?**

*Yes*, if it was created for you by your instructor, for example in a
GitHub organization of your course. In this case, the repository is
probably private and only you and your instructor can see it. It should
have a name like `Project[...]_[YourID]`.

*No*, if you read a Template repository on GitHub. In this case, it is
probably public on the GitHub page of an instructor and everyone can see
it. Create your own repository by clicking on the green button *Use this
template* and create your own repository and continue from there.

## Have git and GitHub working together

**Note:** If you just want to work locally on your computer without
having your own GitHub repository, you can just clone it from GitHub
following the instruction below, but you will not be able to push your
work to the repository.

If you do this Project as part of a course, your instructor wants you to
work on your own repository and commit and push your work to the
repository on GitHub for review.

**To that end, you need to have a GitHub account.** You should already
have one if this repository has your ID in its name.

**You need to link your local git installation with your GitHub
account.**  
A fast way is documented in the excellent [Happy Git and GitHub for the
useR](https://happygitwithr.com/) online book by Jenny Bryan. Here is a
short summary:

1.  [Introduce yourself to git](https://happygitwithr.com/hello-git):
    Use the [`usethis`](https://usethis.r-lib.org/) package to set your
    name and email for git. This is important for all your commits.

    In the CONSOLE type:

    ``` r
    install.packages("usethis")  # Only once
    usethis::use_git_config(user.name = "Your Name", user.email = "your@email-used-for-github.account")
    ```

2.  [Personal access token for
    HTTPS](https://happygitwithr.com/https-pat): This is a way that your
    git installation can authenticate at GitHub for pushing information
    to it. Another small package helps with this called
    [`gitcreds`](https://gitcreds.r-lib.org/).

    In the CONSOLE type:

    ``` r
    usethis::create_github_token()  # This opens a browser window at GitHub
    ```

    Follow the instructions there. You can create tokens with expiration
    date or without. If you use an expiration date you have to do the
    same process again after expiration. If you select *No expiration*,
    be aware that someone using your computer years after today can
    access your GitHub account. There are several checkboxes. Leave the
    default and click *Generate Token*. Copy the token (a series of
    letters like a password) to the clipboard. Then in the CONSOLE in
    Positron type:

    ``` r
    gitcreds::gitcreds_set()  # This opens a browser window at GitHub
    ```

    A dialog appears in the CONSOLE. Paste in your token and hit ENTER.
    Now your git installation can authenticate at GitHub and you can
    push your work to your repository on GitHub. Try it using the
    workflow below. \[Note: If you wonder why you did not need to
    install the `gitcreds` package, it is because it is already
    installed as a dependency of the `usethis` package.\]

## Clone your repository and move into the workflow

The **Tasks and Questions** for this Project are written in the file
[INSTRUCTIONS.md](INSTRUCTIONS.md). Open that in the browser and follow
the instructions there.

- **Workflow** for the Project:
  1.  Clone the Project Repository to your local computer. (The git
      command for this is `git clone <URL>` where `<URL>` is the URL of
      your repository you created on.) In Positron you can do this via
      *File* or *+ New* -\> *New Folder from git*Version Control -\>
      Git\* and then enter the URL of your repository. You find the URL
      on the Projekt Website on GitHub. This creates a folder on your
      computer with all the files of the repository. Watch out for the
      green button *\<\> Code*, click on it and copy the URL from
      there.  
      **Organization Advice:** Create a folder for all projects on your
      computer. In this folder you will have all the subfolders created
      by the cloning of the project repositories.
  2.  Work on the Project’s main quarto markdown document
      (`Project[...].qmd`-file). You find it in the File Explorer of
      Positron (in the vertical toolbar on the left). Click on it to
      open it in the editor.  
      What does *working on the project document* mean?
      - You structure the markdown document with headlines
      - You draft plain markdown text to explain what you analyze and
        report your results
      - You create code cells and write code either for loading
        packages, importing data, making computations, and producing
        visualizations and tables in the output document
      - While you do this you work a lot using the CONSOLE in which you
        execute part of your written code line by line to test it or
        modify it to explore other options until you develop something
        which goes into your file. (*Ctrl + ENTER* to send a line in
        your code cell to the CONSOLE becomes a usual thing to do.)
  3.  (Sometimes/Optionally) Create an additional script-file, for
      example for downloading data, or performing lengthy calculation
      and creating intermediate datasets.
  4.  Click the *Preview* button on above the script. This renders the
      main Project’s quarto markdown document and creates a HTML-file
      with your Project Report (More information in the [Guide for
      Positron](https://quarto.org/docs/get-started/hello/Positron.html)).
      On the command line it could be done by
      `quarto render Project[...].qmd` in the TERMINAL.
  5.  You repeat step 2.-4. and repeatedly check if your (local)
      HTML-output looks good. If you receive an error, you have to solve
      them until it renders successfully again. You repeat until you are
      done for your work session. Ideally, you leave the document such
      that it renders well and with a short list of problems to solve
      and next steps to do.
  6.  After a work session it is a good idea to bring your local
      repository **in sync with the remote repository on GitHub**. For
      this open the *Source Control* tab in Positron (on the vertical
      toolbar on the left). There you see all the files which were
      changed since your last commit. The command line commands for
      bringing your local repository in sync with the remote repository
      are `git add [file1] [file2] ...` to add files to the staging
      area, then `git commit -m "Your commit message"` to create a
      commit locally, and finally `git push` to push the commit to the
      remote repository. However, in Positron you can do this via the
      nice interface in the *Source Control* tab.
      1.  **Add files to the staging area:** Hover over the file
          `Project[...].qmd`, a *+* appears, click it and the file will
          move to the *Staged changes*. Do the same with the
          HTML-file.  
          **Explanations:** The files marked with an *M* are *Modified
          files*. The files marked with a *U* are *Untracked files*.
          Your file `Project[...].qmd` was *M*, your file
          `Project[...].html` was *U*. In the staging area both are *A*
          for *Added*.
      2.  **Create a commit:** In the input field above the list of
          staged changes enter a meaningful commit message, for example
          “Solved until step X.”. Then click on the big *Commit*
          button.  
          **Explanations:** This creates a local commit on you machine.
          Below the *CHANGES* area you see a *GRAPH* are. This list all
          your commits with the most recent at the top. You can think of
          a *commit* as saving several files in a folder at the same
          time. You can go back to old commits. This would reverse all
          files back to the earlier state.
      3.  **Push the commit to the remote repository:** Two ways. Either
          click on the *…* for *More actions* by hovering over the
          *CHANGES* and then select *Pull*, or in the GRAPH area find
          the *Push* symbol (up arrow) and click it. **Explanations:**
          This pushes your local commit to the remote repository on
          GitHub. You can check this by going to the website of your
          repository on GitHub and refresh the page. You should see your
          newly committed and pushed files there.
  7.  Repeat steps 2.-6. until your Project Report is final. Commit it
      with message “Final”.

  Now you have a repository with your Project Report on GitHub.
