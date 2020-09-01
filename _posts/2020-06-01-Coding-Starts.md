---
toc: true
layout: post
description: End of Community Bonding Period and official start of Coding Period according to the GSoC Timeline
categories: [coding, communication]
title: Coding Officially Starts!
image: images/code.png
search_exclude: true
comments: true
---

### Updates

The community bonding period ends today. During this time, we established a working and communication strategy with the mentors. TMVA developers have [Mattermost](https://mattermost.com/), an open-source, self-hostable online chat and communication service, similar to slack. A meeting will be organized every Friday for discussing the progress and doubts with the mentors throughout the program. Thanks to Omar, who provided me access to a GPU enabled server required for testing the interface.

There was a welcome meeting organized by the CERN-HSF GSoC team, lead by the admin, Dr. Andrei Gheata. The admins provided some insight on the past programs, and the experience they have had with GSoC students earlier. He adviced us to comply with the rules as members of the CERN community stringently. We had a small discussion about the milestones in the coding period and how one should approach the coming phases in the program.

I also managed to connect with Stefan Wunsch (ROOT contributor) who developed the PyKeras interface in TMVA-ROOT. His help and suggestions can prove to be valuable, since we plan to create a PyKeras like interface, but for PyTorch.


### Building ROOT for Development

Coming to the development based issues, one common problem with **ROOT** is that it is **arduous to install** – if you build from source, it’s a several hour task on a single core (clean build on my local machine with 4 cores at 8 threads took more than 2 hrs). It has gotten much better in the past few years, but there's still scope for improvement. This is **especially true for Python**; ROOT is linked to your distro’s Python as of ROOT 6.22; but the common rule for using Python is “don’t touch your system Python” - hence modern Python users should be in a virtual environment, and for that ROOT requires the system site-packages option be enabled, which is not always ideal. 

Like every other machine learning person, you can use the **Anaconda Python distribution**, which is the most popular scientific distribution of Python and massively successful for environment management.

**But, the general rule even for people who build ROOT themselves has been: DON'T**.

```shell
# Note: Ensure that all conda environments are deactivated.
git clone https://github.com/root-project/root.git
mkdir build_root install_root && cd build_root

# Standard
cmake -DCMAKE_INSTALL_PREFIX=../install_root ../root
# OR
# Faster Build with limited options
cmake -DCMAKE_INSTALL_PREFIX=../install_root -Droofit=OFF -Dccache=ON ../root

# If you have n=4 cores available for compilation
cmake --build . -- install -j4
source ../install_root/bin/thisroot.sh 

# Setup an alias for activating root.
alias thisroot='source /Users/gollum/Desktop/Work/gsoc/install_root/bin/thisroot.sh'
```
<figcaption><b><center>Steps to get up & runnning!</center></b></figcaption>

One may disable parts of root that you don't need, e.g., `roofit`, just add the `-Droofit=OFF` flag to cmake.
Some other tricks to improve the build cycle is to use ccache and call the build with `cmake /path/to/source -Dccache=ON`.

Although the build takes about 2 hours or so for the first time, the incremental builds are generally much much faster. This is because, by default cmake rebuilds only what is required and a clean build is not triggered everytime changes are made.

<img alt="Can't See? Something went wrong!" src="{{site.baseurl}}/images/cling.jpg">
<figcaption><b><center>Working with ROOT CLING</center></b></figcaption>


### WIP PR

I started coding in the community bonding period to get a head start. I've implemented `MethodPyTorch.h` header which contains macro definitions and method declarations to be shared between the source files and invoked later. Also I've set-up structure of the project with `cmake`.

A Draft Pull Request marking the start of the official coding period and to track the progress is now available [here](https://github.com/root-project/root/pull/5757).

<a href="https://github.com/root-project/root/pull/5757">
<img alt="Can't See? Something went wrong!" src="{{site.baseurl}}/images/baby_pr.jpg" width="60%">
</a>
<br/>
Although for now the PR is genuinely in the baby stage and a plethora of things are to be implemented afore some genuine feedback can be provided, feel free at any time for any kind of suggestions and comments. It would be extremely helpful and I'll endeavor my best to incorporate the suggested improvements.