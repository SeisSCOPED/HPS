# Preliminary Coding Work

We welcome all types of learners to our event, from professional software developers to people just getting started. Based on previous experience, participants gain more from our hackweeks when they arrive having a preliminary understand of some of the foundational tools of data science workflows. These skills include knowing how to:

* Navigate a [Jupyter Notebook](https://jupyter.org/) environment
* Conduct file management, text editing and other basic tasks from a command line interface
* Add and commit changes in Git, and push and pull content from GitHub
* Create simple scientific workflows in Python

## {{hackweek}} Software Carpentry Session

We strongly encourage participants to review this two-day recorded fundamentals of Python and open-source workflows crash course ([Software Carpentry Schedule](swc)) in advance of the CyberTraining. You may choose whichever topics you'd like to brush up on or learn. 


## Required setup

```{attention}
Please make sure to find some time to go through the below material before
the hackweek.
```


### Shell Scripting

Everyone attending will be exposed to shell scripting and it will be necessary to have a basic skills. The [Software Carpentries Shell Novice](https://swcarpentry.github.io/shell-novice/) have necessary educational materials to be comfortable for the workshop.

### GitHub Account

Everyone attending {{ hackweek }} will require obtaining a GitHub account.
Visit our [GitHub instruction page](github) to learn how!

### Slack Account

All of our communication throughout the hackweek will be done using the
{{ '[`{hackweek}` Slack workspace]({url})'.format(hackweek=hackweek, url=slack_workspace_url)}}.
With your invite to the hackweek, you should also have received a separate
email to join the Slack workspace. Upon accepting the invite, please take a moment to
[complete your Slack profile](https://slack.com/help/articles/204092246-Edit-your-profile).
Having your name and picture with your Slack account helps us and your peers
to identify you on Slack and builds a more personal community throughout
the week.

### JupyterHub

We will offer all tutorials within the Jupyter Hub computing environment.
Visit our [Introduction to Jupyter Hub](jupyterhub) page to learn more!

### Git

All content of the hackweek will be shared via GitHub and interacting with the
website will be done via the `git` command. Visit [Setting up the `git` command](git)
to learn how to configure that!

## Optional setup

### Python
Dive deeper into how [Python is managed and installed](python) on the JupyterHub
and how you can install that on your personal machine.


## Software and Editing Carpentries:
- [Git](https://swcarpentry.github.io/git-novice/): code version control 
- [Python](http://swcarpentry.github.io/python-novice-gapminder/): coding fundamentals
- [Latex](https://www.overleaf.com/learn/latex/Learn_LaTeX_in_30_minutes): Manuscript editing preferred scripting language
- [Jypyter Notebooks](https://www.earthdatascience.org/courses/intro-to-earth-data-science/open-reproducible-science/jupyter-python/get-started-with-jupyter-notebook-for-python/): jupyter notebook introduction
- [Conda, Mamba, environment](https://astrobiomike.github.io/unix/conda-intro): nice intro from a biologist on the basics of conda, mamba, and creating a computing environment.


## Basic Seismology
- Please watch this [video](https://www.youtube.com/watch?v=kFwdjfiK4gk) of an introduction to Opbsy.
- Install Obspy using conda: ```conda install -c conda-forge obspy```
- [Reading seismograms](https://docs.obspy.org/tutorial/code_snippets/reading_seismograms.html)
- [Plot waveforms](https://docs.obspy.org/tutorial/code_snippets/waveform_plotting_tutorial.html)
- [Filter data](https://docs.obspy.org/tutorial/code_snippets/filtering_seismograms.html)
- [Spectrograms](https://docs.obspy.org/tutorial/code_snippets/plotting_spectrograms.html)
- [Focal mechanism](https://www.ocean.washington.edu/courses/oc410/reading/Focal_mechanism_primer.pdf)

## Intermediate Seismology
- [Practice downloading and plotting](https://krischer.github.io/seismo_live_build/html/ObsPy/07_Basic_Processing_Exercise_solution_wrapper.html)
- [Practice looking at earthquake](https://krischer.github.io/seismo_live_build/html/ObsPy/08_Exercise__2008_MtCarmel_Earthquake_and_Aftershock_Series_solution_wrapper.html)

Fantastic est of video tutorials by the ROSES program. Note that every year has slightly different topics.
- Offering in [2020](https://connect.agu.org/seismology/roses/roses2020materials) and their [Github](https://github.com/roseseismo/roses2020)
- Offering in [2021](https://connect.agu.org/seismology/roses/roses2021materials), and their [Github](https://github.com/roseseismo/roses2021)
  

## Experts in Computing and Seismomlogy
- Cloud computing video tutorials:
    - https://www.youtube.com/watch?v=0hGoK1SdBm4: get started on AWS-EC2 (by our own Julian Schmidt!)