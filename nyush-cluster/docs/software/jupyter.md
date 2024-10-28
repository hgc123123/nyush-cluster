## Introduction

JupyterLab is the latest web-based interactive development environment for notebooks, code, and data. 
Its flexible interface allows users to configure and arrange workflows in data science, scientific computing, computational journalism, and machine learning. 
A modular design invites extensions to expand and enrich functionality.

## Version

| Cluster | Version   | Module                    |
|:--------|:----------|:-------------------------:|
| nyushc  | 7.0.6     | jupyter/7.0.6-cuda-11.4.3 |

## Launch Jupyter

Setup an [Open OnDemand (OOD)](https://ood.shanghai.nyu.edu) portal web server.

To start the session, please go to `My Interactive Apps` in the top menu bar and select `Jupyter` in the left-hand panel.

![](figures/ondemand-interactive-jupyter-config.png){: style="width:90%;" .center}

Allocate appropriate resources and click `Launch`.

An info card for the RStudio Server will be added to `My Interactive Sessions`, and during start,
it will change its state from `Queued` to `Starting` to `Running`. Depending on the app, resources allocated and
current cluster usage, this will take a couple of seconds.


![](figures/ondemand-interactive-jupyter-queued.png){: style="width:90%;" .center}


![](figures/ondemand-interactive-jupyter-starting.png){: style="width:90%;" .center}


![](figures/ondemand-interactive-jupyter-running.png){: style="width:90%;" .center}


When in the final state (`Running`), one can directly connect to the JupyterLab
to get an interactive session by clicking `Connect to Jupyter`:

![](figures/ondemand-interactive-jupyter-session.png){: style="width:90%;" .center}

Open JupyterLab

![](figures/ondemand-interactive-jupyterlab-open.png){: style="width:90%;" .center}

![](figures/ondemand-interactive-jupyterlab-session.png){: style="width:90%;" .center}
