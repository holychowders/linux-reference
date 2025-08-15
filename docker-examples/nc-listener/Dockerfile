FROM ubuntu
#SHELL ["/bin/sh", "-c"]
RUN apt update; apt upgrade -y; apt install -y iproute2 netcat-traditional tcpdump tmux

#WORKDIR /app
COPY ./nc-listener /nc-listener

#CMD ["nc", "-l", "1234"]
CMD ["/nc-listener"]
