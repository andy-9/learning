FROM ubuntu
RUN apt-get update
RUN apt-get install -y 2to3 python2-minimal python2 dh-python python-is-python3 python3-pip
RUN pip3 install flask
COPY app.py /opt/app.py
ENTRYPOINT FLASK_APP=/opt/app.py python -m flask run --host=0.0.0.0
