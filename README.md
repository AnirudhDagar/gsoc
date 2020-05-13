# GSoC Blog

![](https://github.com/AnirudhDagar/gsoc/workflows/CI/badge.svg) 
![](https://github.com/AnirudhDagar/gsoc/workflows/GH-Pages%20Status/badge.svg)

## CERN-HSF

**Building Root on MacOS X (Without Anaconda)**:
```bash
git clone https://github.com/root-project/root.git
cd root
mkdir _build
cd _build
cmake -DPYTHON_INCLUDE_DIR=/usr/local/Cellar/python/3.7.7/Frameworks/Python.framework/Versions/3.7/include/python3.7m -DPYTHON_EXECUTABLE=/usr/local/opt/python/libexec/bin/python -DINTERFACE_PYTHON=ON ..
make -j4
```

**Some helpful ROOT/TMVA links**:

* [ROOT Homepage](https://root.cern.ch/)
* [ROOT Forums](https://root-forum.cern.ch/)
* [Inter-Experimental LHC Machine Learning Working Group (IML)](https://iml.web.cern.ch/)
* [Building ROOT](https://root.cern/building-root)
* [TMVA Tutorials](https://github.com/lmoneta/tmva-tutorial)
* [Stefan Wunsch's Tutorial Recording on Keras and TMVA interfaces](https://cds.cern.ch/record/2256883?ln=en)
* [Stefan Wunsch's IML Keras Workshop Code](https://github.com/stwunsch/iml_keras_workshop)
* [Stefan Wunsch's IML Keras Workshop Slides](https://indico.cern.ch/event/595059/contributions/2522193/attachments/1430921/2197986/slides_iml_keras_workshop.pdf)

The website is powered by the [fastpages](https://github.com/fastai/fastpages) template and the minima theme.
