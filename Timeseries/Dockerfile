# Build an image that can do training and inference in SageMaker
# This is a Python 3.8 image that uses the nginx, gunicorn, flask stack
# for serving inferences in a stable way.

FROM ubuntu:20.04

MAINTAINER Amazon AI <sage-learner@amazon.com>

ENV TZ=Asia/Kolkata
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN apt-get -y update
RUN apt-get install -y build-essential 
RUN apt-get install -y --no-install-recommends \
         wget \
         python3.6 \
         python3-dev \
         python3-pip \
         nginx \
         ca-certificates \
         curl \
         wget \
         git \
         libopencv-dev \
         openssh-client \
         openssh-server \         
    && rm -rf /var/lib/apt/lists/*

RUN ln -s /usr/bin/python3.6 /usr/bin/python
#RUN ln -s /usr/bin/pip3 /usr/bin/pip

# Here we get all python packages.
# There's substantial overlap between scipy and numpy that we eliminate by
# linking them together. Likewise, pip leaves the install caches populated which uses
# a significant amount of space. These optimizations save a fair amount of space in the
# image, which reduces start up time.
COPY requirements.txt ./
RUN pip3 --no-cache-dir install -r requirements.txt

# Set some environment variables. PYTHONUNBUFFERED keeps Python from buffering our standard
# output stream, which means that logs can be delivered to the user quickly. PYTHONDONTWRITEBYTECODE
# keeps Python from writing the .pyc files which are unnecessary in this case. We also update
# PATH so that the train and serve programs are found when the container is invoked.

ENV PYTHONUNBUFFERED=TRUE
ENV PYTHONDONTWRITEBYTECODE=TRUE
#ENV PATH="/opt/program:${PATH}"
ENV LD_LIBRARY_PATH "${LD_LIBRARY_PATH}:/usr/local/lib"
# Set up the program in the image
#RUN pip3 install --no-cache --upgrade \
#    sagemaker-training

# Print all dependencies
RUN echo "Installed Python packages:" && \
    pip3 freeze


# Start a shell to access the container
CMD ["/bin/bash"]