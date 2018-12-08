FROM debian:latest
MAINTAINER https://github.com/uso8000k/

RUN apt-get clean && apt-get update
RUN apt-get install -y git-core sudo make wget pkg-config cpio

VOLUME ["/tmp/images"]

# Add builduser (some task must not root)
RUN useradd -m builduser
RUN echo 'builduser ALL=(ALL:ALL) ALL' >> /etc/sudoers
RUN echo 'export PATH=$PATH:/sbin/:/usr/sbin/' >> /home/builduser/.bashrc
RUN su builduser -c "git config --global user.email example@example.com; git config --global user.name example"
RUN su builduser -c "cd /home/builduser/; git clone https://github.com/opencomputeproject/onie"
RUN ln -s /home/builduser/onie /opt/onie
RUN chsh -s /bin/bash builduser

RUN cd /opt/onie/build-config && make debian-prepare-build-host

COPY scripts/build /usr/local/bin/
RUN  chmod 755 /usr/local/bin/build 

CMD echo 'Example: $ docker run --rm --name onie -it -v <dir>:/tmp/images <container> build <onie_arch>'
