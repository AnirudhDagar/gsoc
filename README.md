![](https://github.com/AnirudhDagar/gsoc/workflows/CI/badge.svg) 
![](https://github.com/AnirudhDagar/gsoc/workflows/GH-Pages%20Status/badge.svg)

![](images/GSoC-logo-homepage.svg)

## CERN-HSF
This is the source repository for my GSoC blog and I've kept some useful stuff in the readme as well which I may refer along the way for the project.

**Building Root on MacOS X (Without Anaconda)**:

```bash
#Note: Ensure that all conda environments are deactivated.
git clone https://github.com/root-project/root.git
mkdir build_root install_root && cd build_root
cmake -DCMAKE_INSTALL_PREFIX=../install_root ../root # && check cmake configuration output for warnings or errors
cmake --build . -- install -j4 # if you have 4 cores available for compilation
source ../install_root/bin/thisroot.sh 

#Setup an alias for this
alias thisroot='source /Users/gollum/Desktop/Work/gsoc/install_root/bin/thisroot.sh'
```

**Some helpful ROOT/TMVA links**:

* [ROOT Homepage](https://root.cern.ch/)
* [ROOT Forums](https://root-forum.cern.ch/)
* [Inter-Experimental LHC Machine Learning Working Group (IML)](https://iml.web.cern.ch/)
* [Building ROOT](https://root.cern/building-root)
* [TMVA User Guide](https://root.cern.ch/download/doc/tmva/TMVAUsersGuide.pdf)
* [TMVA Tutorials](https://github.com/lmoneta/tmva-tutorial)
* [Stefan Wunsch's Tutorial Recording on Keras and TMVA interfaces](https://cds.cern.ch/record/2256883?ln=en)
* [Stefan Wunsch's IML Keras Workshop Code](https://github.com/stwunsch/iml_keras_workshop)
* [Stefan Wunsch's IML Keras Workshop Slides](https://indico.cern.ch/event/595059/contributions/2522193/attachments/1430921/2197986/slides_iml_keras_workshop.pdf)

Other Useful blog links about TMVA:
* [Surya's Blog](https://surya2191997.github.io/)
* [RaviKiran's Blog](https://www.sravikiran.com/GSOC18/)
* [Vladimir's Blog](https://ilievskiv.github.io/blog/2017-05-05-gsoc-start/)
* [Saurav's Blog](https://sshekh.github.io/blog/gsoc/index)
* [Abhinav's Blog](https://amoudgl.github.io/blog/gsoc-conclusion/)
* [Simon's Blog](http://simonpf.github.io/gsoc/)


The website is powered by the [fastpages](https://github.com/fastai/fastpages) template and the minima theme. Build locally with docker installed and running using `make server`.
