# How to do a Homework Project with your own Repository


- [Needed infrastructure](#needed-infrastructure)
- [Your own GitHub Repository](#your-own-github-repository)
- [Have git and GitHub working
  together](#have-git-and-github-working-together)
- [Clone your repository and move into the
  workflow](#clone-your-repository-and-move-into-the-workflow)
- [Should I use AI to do this
  Homework?](#should-i-use-ai-to-do-this-homework)
- [Solutions for common problems](#solutions-for-common-problems)

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
- *Integrated Development Environment* (IDE): The instructor uses
  [Positron](https://Positron.posit.co/). All instructions here are
  based on this IDE. In the past the instructor used RStudio. People
  using python are often used to Visual Studio Code. As Positron and
  Visual Studio Code are both built on Code - OSS, many things are
  similar. In prinple, you can use whatever you want.

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
having your own GitHub repository, you can clone it from GitHub
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

The **Tasks and Questions** for this Project are written in the
[INSTRUCTIONS](INSTRUCTIONS.md). Open that in the browser and follow the
instructions there.

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
  2.  Work on the Project’s main quarto markdown document (typically
      `report.qmd`-file). You find it in the File Explorer of Positron
      (in the vertical toolbar on the left). Click on it to open it in
      the editor.  
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
  4.  Click the *Preview* button while being in the main quarto markdown
      document (typically `report.qmd`-file). This renders the main
      Project’s quarto markdown document and creates a HTML-file with
      your Project Report (More information in the [Guide for
      Positron](https://quarto.org/docs/get-started/hello/Positron.html)).  
      Note: It could also be done from the command line by
      `quarto render report.qmd` in the TERMINAL.
  5.  You repeat step 2.-4. and repeatedly check if your (local)
      HTML-output looks good. If you receive an error, you have to solve
      them until it renders successfully again. You repeat until you are
      done for this work session. Ideally, you leave the document such
      that it renders well and with a short list of problems to solve
      and next steps to do.
  6.  After a work session, it is a good idea to bring your local
      repository **in sync with the remote repository on GitHub**. To
      that end, open the *Source Control* tab in Positron (on the
      vertical toolbar on the left). There you see all the files which
      were changed since your last commit. The command line commands for
      bringing your local repository in sync with the remote repository
      are `git add [file1] [file2] ...` to add files to the staging
      area, then `git commit -m "Your commit message"` to create a
      commit locally, and finally `git push` to push the commit to the
      remote repository. However, in Positron you can do this via the
      nice interface in the *Source Control* tab.
      1.  **Add files to the staging area:** Click on a file
          (e.g. `report.qmd`), a *+* appears, click it and the file will
          move to the *Staged changes*. Do the same with the
          HTML-file.  
          **Explanations:** The files marked with an *M* are *Modified
          files*. The files marked with a *U* are *Untracked files*.
          Your file `report.qmd` was *M*, your file `report.html` was
          *U*. In *Staged Changes* both are *A* for *Added*.
      2.  **Create a commit:** In the input field above the list of
          staged changes enter a meaningful commit message, for example
          “Solved until step X.”. Then click on the big *Commit*
          button.  
          **Explanations:** This creates a local commit on you machine.
          Below the *CHANGES* area you see *GRAPH*. This lists all
          commit with the most recent at the top. You can think of a
          *commit* as saving several files in a folder at the same time.
          You can go back to old commits. This would reverse all files
          back to the earlier state.
      3.  **Push the commit to the remote repository:** Two ways: Either
          click on the *…* for *More actions* by hovering over the
          *CHANGES* and then select *Push*, or in the GRAPH area find
          the *Push* symbol (up arrow) and click it. **Explanations:**
          This pushes your local commit to the remote repository on
          GitHub. You can check this by going to the website of your
          repository on GitHub and refresh the page. You should see your
          newly committed and pushed files there.
  7.  Repeat steps 2.-6. until your Project Report is final. Commit it
      with message “Final”.

  Now you have a repository with your Project Report on GitHub. Go to
  the website of your repository on GitHub and check that everything is
  there!

## Should I use AI to do this Homework?

Use it for learning. This Homework Project is intentionally not designed
as an assignment which you need solve and hand in within a week. It
mimics a real data science project but with much more guidance.

Probably it is possible to have an AI agent do this Homework Project
completely without you having to look at it. It is interesting to try
this. However, if you use it just fulfilling requirements you will learn
almost nothing about doing data science yourself. Further on, it is
almost sure that your instructor will recognize that it is not your work
and that you are not engaging with the content.

Use AI in a way that makes you better in understanding what is happening
and how data wrangling, visualization, and analysis is done.

Finally: You have to take full responsibility of the content you deliver
even if it is AI generated. If you do not know what you handed in, it
has no value for no one.

## Solutions for common problems

- **I cannot push, it gets rejected.** Read the Positron error message,
  also check the command output. It is likely that there is new content
  on the remote repository on GitHub which is not in sync with your
  local repository. (Maybe you or your instructor has changed a file on
  GitHub in the browser and thereby created a new commit there?) To
  solve this, you have to pull the new content from GitHub to your local
  repository and then push again. In Positron, you can do this via the
  *Source Control* tab. Click on the *…* for *More actions* by hovering
  over the *CHANGES* and then select *Pull*. Alternatively, you can find
  the *Pull* button in the GRAPH area. This pulls the new content from
  GitHub to your local repository. Now you can push again.
- **You have *divergent branches* and need to specify how to reconcile
  them.** Probably this was the first time you pulled and you need to
  specify for this repository how git should handle it. Read the error
  message carefully including the command output. It tells you that you
  can do one of three things. I recommend you copy
  `git config pull.rebase false` and paste it in the TERMINAL and hit
  ENTER. This will set the default for this repository to merge the
  branches when pulling. You can also set it to rebase or fast-forward
  only. If you do not understand what that means, just go with
  `git config pull.rebase false` for now. It is anyway not very relevant
  for a small one-author project like this. After that, you can pull
  again and should not receive this message again.
- **I have pulled and now I have a merge conflict.** This means that
  there are changes in the same file on GitHub and on your local
  repository which cannot be automatically merged. You have to solve
  this manually by going through the conflicted files. In Positron, you
  can do this via the *Source Control* tab. Click on the file with the
  merge conflict. It will show you the conflicting changes and you can
  decide which one to keep. Remove all the `<<<`, `===`, and `>>>` and
  make it as it should be. That is called *solving the conflict*. After
  solving the conflict, you have to add the file to the staging area
  again and then commit and push as usual.
- **How can I test that my submission on GitHub is as my instructor
  wants it?** Sometimes, students forget to `git add` and (most
  importantly) `git push` all necessary files. This can imply that the
  student did everything fine and they see their report but when
  instructors clone the repository it is incomplete or not even touched.
  How can you check? Here is a safe test to check what instructors can
  see:  
  Go to the repository on GitHub. Clone your repository anew to a
  temporary directory on your computer (for example your Download
  folder). Open the HTML file from this newly cloned directory. Check if
  it looks good. This is what instructors will do. Then you can delete
  the temporary directory again.  
  **LEARNING:** What is on your computer is not the same to what is on
  GitHub. This is not unusual in git workflows. Always check what is in
  GitHub if you are unsure!
- **I want to change the visibility of my repository to public.** If
  your repository is part of a a course organization where you only have
  write access, you can not make it public yourself. You need admin
  rights. Ask the instructor to do it. If you have admin rights go to
  the settings and scroll down to the so-called “Danger Zone”.  
  Be aware that if you go public, everyone can see your repository and
  all its content. Before going public make the repository ready for it.
  Two things are important:
  1.  Check the [LICENSE](LICENSE.md) and do what is required.
  2.  Check if you accidentally have files in your repository which you
      are not allowed or do do not want to share with the outside world.
      There should be no confidential personal data like passwords, API
      keys, or similar. If your homework had data provided for you for
      internal use only, the raw data should not be on GitHub (it can
      still be on your local computer). You should delete the files from
      GitHub, but also from the history of the repository. This is not
      as simple as one may think, because it implies to rewrite or
      delete history and for large collaborative projects this is a
      problematic. For this small project it does not matter. Below is
      an instruction how you can reinitiallize git, commit only the
      files which should be online and than force git to push this new
      history to GitHub.
- **I need to remove files from the history of the repository.** If you
  have files in your repository which you do not want to share with the
  outside world, you can reinitialize git, commit only what is needed,
  and force push this new short history to GitHub. This is a bit of a
  hack, but it works for small projects.
  1.  Make a backup of your local repository on your computer by copying
      the whole folder to another location. Ideally you will not need to
      look into the backup again, but it is better to have it just in
      case, because deleting history is a bit of a hack and you do not
      want to lose your work.
  2.  In the original folder, delete the `.git`-folder. This deletes all
      git history and all commits recorded locally. The folder is a
      hidden folder. You have to enable the option to see hidden files
      in your file explorer to see it. You can also do this from the
      command line by `rm -rf .git` in the TERMINAL while being in the
      original folder. *Always be cautious with the `rm -rf` command*!
      Make sure you are in the right folder and that you have a backup
      before doing this! It deletes the folder and all subfolders
      without warning, and it cannot be undone!  
      Once you have deleted the `.git`-folder, your folder is no longer
      a git repository. It has also lost its connection to GitHub! We
      will re-establish both.  
  3.  Initialize a new git repository locally by `git init` in the
      TERMINAL while being in the original folder.
  4.  Add all files which should be online to the staging area by
      `git add [file1] [file2] ...` or `git add .` for all files. Do not
      add the files which should not be online! They can stay untracked
      and will not be in the new history.
  5.  Create a new commit by `git commit -m "Initial commit"` in the
      TERMINAL. Now, you have re-estabilshed a git repository locally
      with a single commit containing only the files you want to share.
  6.  Re-establish the connection to GitHub. To that end, you have to
      add the remote repository again by `git remote add origin <URL>`
      where `<URL>` is the URL of your repository on GitHub. You find
      the URL on the Projekt Website on GitHub. Watch out for the green
      button *\<\> Code*, click on it and copy the URL from there. It is
      the same button you use to clone your repository.
  7.  Force push this new history to GitHub by
      `git push --force origin main  --set-upstream origin main` in the
      TERMINAL. Now everything should look alright on GitHub. Check it
      by going to the website of your repository on GitHub and refresh
      the page. You should see only the files you wanted to share and no
      history except the new initial commit.
  8.  A final step connects your `main` branch to the remote `main`
      branch: Do `git push --set-upstream origin main`. Only if you do
      this you can continue to push new stuff as usual.
