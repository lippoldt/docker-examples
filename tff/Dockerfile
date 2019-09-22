FROM tensorflow/tensorflow:nightly-gpu-py3 
RUN apt-get update && apt-get install -y python3-dev python3-pip nano git graphviz
RUN pip3 install --upgrade pip
RUN pip3 install --upgrade jupyter tensorflow_federated pydot graphviz
RUN python3 -c "import tensorflow_federated as tff; tff.federated_computation(lambda: 'Hello World')()"
