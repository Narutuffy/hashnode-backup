## Optimize Your Data Science Environment

I have entered the world of data science in the last couple of years, coming from the engineering and devops side. I think the perspective of an engineer on the data science world is very different than someone who started as a data scientist from the first place. I would like to share my 3 engineering tips that improved my data science work and I hope it will do the same to you.

### Use Docker 🚢

I must admit, I hate the package management of Python and all its virtual environments. Coming from more evolved ecosystems (such as NodeJS), it’s hard to get used to it. On top of it, if you are doing deep learning you are obviously utilizing the GPU so you need to install Nvidia drivers, CUDA and what not.

I do not use conda and actually I do not have it installed, I use Docker for my data science projects. I use [Deepo](https://hub.docker.com/r/ufoym/deepo/) as my base image as it comes pre-built with everything you need to start running deep learning workloads (Tensorflow, Keras, Python 3, CUDA, drivers, etc). Instead of the traditional way of writing a Dockerfile, I simply initialize a new container like this: `docker run — runtime=nvidia — name container-name -p 8888:8888 -v directory/to/share:/container/path -d ufoym/deepo:all-jupyter jupyter notebook — no-browser — ip=0.0.0.0 — allow-root — notebook-dir=’/container/path’`.
This command provisions a new container and sharing a directory with the container so you can share your data and code for example. In addition, it exposes port 8888 for accessing jupyter from the browser. You can now access the container’s bash with the following: `docker exec -ti container-name bash`. Everything you do inside the container will remain there, any package or software you install. After a batch of changes I can commit all the changes and push the image so I can use it wherever I want. The only thing you need on the host system is Docker installed, nothing more.

### Run Jobs Remotely👩‍🚀

Deep learning processes such as training or hyper tuning can take so much time. I have a strong laptop with an awesome GPU so the temptation to run it all locally is strong. Ever since I thought about using Docker it so much easier to run everything on cloud but doing many quick iterations locally. This way I can duplicate my work environment easily by just pulling the latest Docker.

Running jobs remotely gives me access to a much superior hardware such as Tesla graphic cards and insane amount of CPU and memory at my service (so expensive 😓). The major benefit is that I have my computer for other tasks that I can do in between the workloads. If you want to go extreme, jupyter has some slack integrations so it can notify you on slack when everything is done so you do not have to check yourself.

### Parallelize👯

Even with one GPU, you can run multiple training workloads as long as you have enough memory on your GPU. It can saves your hours of processing and can shorten your iterations drastically especially when it comes to hyper tuning. Talos already created a function which configures Tensorflow to share the GPU with other processed, you can use it or just copy the code: [https://github.com/autonomio/talos/blob/6a4fbfacdbd7a6ebfddd27668761089978cfc053/talos/utils/gpu_utils.py#L1](https://github.com/autonomio/talos/blob/6a4fbfacdbd7a6ebfddd27668761089978cfc053/talos/utils/gpu_utils.py#L1)

All you need is to spawn multiple processes, each process can run a single training workload for example. I love the `multiprocessing` package of Python, it has a [pool](https://docs.python.org/2/library/multiprocessing.html) object which easily lets you spawn other processes.

That’s it! Very straight forward tips which will hopefully save you hours and will make your life easier 😁
