# Intro


In this section, we will review software best practices, open science, reproducible workflows. We will also review how to use HPC resources and Cloud systems.


:::{note}
Draft Material
:::

We will discuss on how to work with various environments

## Local environment. 

To work locally using python and Jupyter notebooks.

⚠️ this content only conerns Linux and MacOS, please reach out to contribute to windows environments.

We recommend using [Visual Studio Code](https://code.visualstudio.com/) for code and notebook editing, running codes and notebooks, editing github repository, interacting with GitHub. It is how we wrote this book!

Either through your OS or using VSCode, open a terminal. The terminal is used to pass commands to the computer. The programming language to send codes to the computer is Shell. BASH (Bourne Again SHell) is one of the implementations of Shell and is used to efficiently perform tasks. Great tutorials on how to get started with Shell are in the [Software Carpentries Lessons](https://swcarpentry.github.io/shell-novice/).


Get to know your hardware. Find out what CPU, GPU, what memory is on your local environment. One that allows to monitor the resources in real time is ``htop``. Install it on your local environment (e.g., use ``brew install htop`` on MacOSX). 

Machine Learning research requires parallelization. Typically, CPUs (Central Processing Units) are plenty sufficient for most ML applications. They are the basics and default hardware of most computers and are particularly practical when handling large data sets for the In/Out operations. 

However, Deep Learning applications are mostly enabled by the much expansive parallelization of GPUs (Graphic Processing Units). GPUs were developed mostly for gaming, and it is why they have become affordable. The advantages of the GPU is the parallelization capabilities. The arrival of a programming language CUDA (Compute Unified Device Architecture) in 2007 enabled dramatic growth in porting heavy computations on GPUs. CPUs and GPUs are composed of multiple **cores**. CPUs typically have between 8 and 96 cores. GPUs typically have between 5,000-10,000 cores.

To watch how the GPU resources are used, use the following command (if installed on your resource):
```bash
    watch nvidia-smi
```

Individual CPUs can compose a **node**. Multiple **nodes** can compose a **cluster**. Clusters can be accessed locally (if you are lucky!) and most often remotely. There are three typical clusters: your local resource (research group), the institutional resource (e.g., Hyak at UW), the HPC center (e.g., Texas Advanced Computing Center), or the Cloud server (e.g., AWS, Azure. GCP). In Cloud computing, a node is an **instance**.


## HPC
HPC (High Performance Computing) typically refers to a system of tightly connected nodes that enables computing large-scale jobs. The software carpentries have tutorials on how to use clusters. See the [Introduction to HPC lessons](https://epcced.github.io/hpc-intro/). These computing architectures enable "Vertical Scaling", meaning adding larger CPU / memory, or I/O resource. 

HPC systems have 1) a compute cluster, 2) a scratch file system (temporary), and 3) a home file systems. Codes are usually in the home, big data is on the scratch file system. Jobs get run on the compute cluster. HPC requires scheduling jobs in a queue, when the resource is available, the jobs get run. It is typical to run big codes on 500-2,000 nodes. 

Institutions may have their own HPC systems. At UW, the system is called [Hyak](https://hyak.uw.edu/).

National HPC resources require specific request for allocation. Requests are typically done using [ACCESS](https://allocations.access-ci.org/) or [TACC](https://www.tacc.utexas.edu/).


HPC can deploy virtual cloud systems to allow horizontal scaling and cloudstore-like file systems {cite:p}`.`.

One typically 1) chooses the HPC resource, and then 2) moves the data there for the computing workflow.

## Cloud

Cloud computing refers to a system of rather loosely connected nodes. There are many cloud providers, though 3 are on the top list: Amazon Web Services (AWS), Google Cloud Platform (GCP), Microstoft Azure Cloud (Azure). Each of these cloud providers offer a free python Hub access.

Cloud centers are distributed around the world, to enable global fast access to computing resoures. The centers are called `regions`.

Usually, storing data on Clouds is the most expensive of geoscience research. One would typically find the 1) data storage/archive, then 2) choose the cloud provider. For optimal I/O performance, it is recommended to choose the `region` where the data is stored for compute.



### Google Colab

If you have a google account, you can access to a free tier GCP instance that uses CPU, or GPU, or TPU (Tensor Processing Unit).

Here is an example of a Google Colab: 
[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1gpRHGtu9s67xntmM0uUtCJSBcSlB9vo0#scrollTo=J7KihpWyh9ed)


### AWS

AWS is the amazon services for cloud. It is the cloud leader.  Chapter 7 details access and usage of these resources.

Their JupyterHub for machine learning is ran out of [Sagemaker Studio](https://aws.amazon.com/sagemaker/). The first 250 hours of use (within the first 2 months) are *free*.

Why use AWS in the geosciences? It stores already lots of [open access data](https://registry.opendata.aws/). AWS also gathers Sagemaker notebooks associated with these open data for machine-learning purpose. See the [notebook catalog](https://registry.opendata.aws/service/sagemaker-studio-lab/usage-examples/).

Cool [geoscience data sets](https://aws.amazon.com/marketplace/search/results?category=ffb6cf06-608c-4b14-a5a9-756f1ccd5725&FULFILLMENT_OPTION_TYPE=DATA_EXCHANGE&CONTRACT_TYPE=OPEN_DATA_LICENSES&DATA_AVAILABLE_THROUGH=S3_OBJECTS&PRICING_MODEL=FREE&filters=FULFILLMENT_OPTION_TYPE%2CCONTRACT_TYPE%2CDATA_AVAILABLE_THROUGH%2CPRICING_MODEL) stored on the S3 (storage service) of AwS are. [Radiant MLHub](https://mlhub.earth/datasets) stores data on S3.

Some specific data set that could be used in this book:

* **Seismic Data**
    - Southern California Seismic Network. [Here](https://aws.amazon.com/marketplace/pp/prodview-c4rk5lxymj43i?sr=0-99&ref_=beagle&applicationId=AWSMPContessa).
    - Distributed Acoustic Sensing (DAS) PoroTomo experiment. [Here](https://aws.amazon.com/marketplace/pp/prodview-qd7w6cbnmssl2?sr=0-41&ref_=beagle&applicationId=AWSMPContessa).
    - OpenEEW: low cost seismometers distributed in populated areas. [Here](https://aws.amazon.com/marketplace/pp/prodview-ot34yes3afyhq?sr=0-1&ref_=beagle&applicationId=AWSMPContessa)


### Azure

Azure is the Microsoft cloud computer. Chapter 7 details access and usage of these resources.

The JupyterHub *free* access of Azure is called the [Planetary Computer](https://planetarycomputer.microsoft.com/).

Cool [data sets](https://planetarycomputer.microsoft.com/catalog) to access directly on Azure that focus on oceans, atmosphere, surface land, demographiscs. Example below:

- [Landsat](https://planetarycomputer.microsoft.com/dataset/group/landsat)