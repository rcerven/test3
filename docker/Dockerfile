FROM quay.io/rcerven_rhtaptest/rcerven-test/testnudging/ttest2:7968786d2db523c3c18bee30134a2cc7ddc63408@sha256:b31c1f8738200d570e35c2a02259c5d84962b1329eb25f89257697d04b755cbc AS nudgedimage
USER 0
RUN echo "nudged image"
RUN mkdir /testdir2
COPY somedir/app.py /testdir2

FROM registry.access.redhat.com/ubi9/python-39:1-197.1725907694

# By default, listen on port 8081
EXPOSE 8081/tcp
ENV FLASK_PORT=8081

# Set the working directory in the container
WORKDIR /projects

# Copy the content of the local src directory to the working directory
COPY . .

# Install any dependencies
RUN \
  if [ -f requirements.txt ]; \
    then pip install -r requirements.txt; \
  elif [ `ls -1q *.txt | wc -l` == 1 ]; \
    then pip install -r *.txt; \
  fi

USER 0
RUN mkdir /testdir
COPY --from=nudgedimage /testdir2/* .

# Specify the command to run on container start
CMD [ "python", "./app.py" ]
