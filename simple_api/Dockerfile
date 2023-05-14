FROM python:2.7-buster
MAINTAINER jules (lyk1719@gmail.com)
RUN apt-get update
RUN apt-get update -y && apt-get install python-dev python3-dev libsasl2-dev python-dev libldap2-dev libssl-dev -y
RUN pip install flask==1.1.2 flask_httpauth==4.1.0 flask_simpleldap python-dotenv==0.14.0
COPY ./ /api/
#VOLUME ${PWD}:/data/
EXPOSE 5000
CMD ["python", "/api/student_age.py"]
