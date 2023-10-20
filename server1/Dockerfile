FROM ubuntu:latest

RUN apt-get update && apt-get install -y openssh-server
RUN useradd -m user1 && echo "user1:password1" | chpasswd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config
RUN service ssh start
EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
