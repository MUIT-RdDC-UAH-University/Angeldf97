FROM ubuntu:20.04

ENV TZ=Europe/Madrid
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update
RUN apt-get install curl -y
RUN apt-get install net-tools -y
RUN apt-get install dnsutils -y
RUN apt-get install firefox -y
#RUN apt-get install midori -y
RUN apt-get install tcpdump -y


EXPOSE 5900
EXPOSE 22

RUN apt-get install x11vnc xvfb -y
RUN echo "exec firefox" > ~/.xinitrc && chmod +x ~/.xinitrc
CMD ["x11vnc", "-create", "-forever"]
