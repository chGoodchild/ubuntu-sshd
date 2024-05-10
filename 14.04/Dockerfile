FROM ubuntu:14.04
MAINTAINER Aleksandar Diklic "https://github.com/rastasheep"

# Update packages and install openssh-server
RUN apt-get update && \
    apt-get install -y openssh-server && \
    mkdir -p /var/run/sshd

# Set up SSH to allow root login with key authentication only and disable password authentication
RUN echo 'root:root' | chpasswd && \
    sed -ri 's/^#?PermitRootLogin\s+.*/PermitRootLogin prohibit-password/' /etc/ssh/sshd_config && \
    sed -ri 's/UsePAM yes/#UsePAM no/g' /etc/ssh/sshd_config && \
    sed -ri 's/^#?PasswordAuthentication\s+.*/PasswordAuthentication no/' /etc/ssh/sshd_config && \
    sed -ri 's/^#?PubkeyAuthentication\s+.*/PubkeyAuthentication yes/' /etc/ssh/sshd_config

COPY authorized_keys /root/.ssh/authorized_keys

# Create the .ssh directory, set permissions, and create an empty authorized_keys file with correct permissions
RUN mkdir -p /root/.ssh && \
    touch /root/.ssh/authorized_keys && \
    chmod 700 /root/.ssh && \
    chmod 600 /root/.ssh/authorized_keys && \
    chown -R root:root /root/.ssh

# Clean up APT when done
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
