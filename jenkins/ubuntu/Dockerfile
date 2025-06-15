FROM ubuntu:latest

# Install OpenSSH server and create directory required by sshd
RUN apt-get update && \
    apt-get install -y openssh-server && \
    mkdir /var/run/sshd

# Create the user, set the password, and setup .ssh directory
RUN useradd -m remote-user && \
    echo "remote-user:G3M13m0b!" | chpasswd && \
    mkdir -p /home/remote-user/.ssh && \
    chmod 700 /home/remote-user/.ssh

# Copy public key for remote-user
COPY remote-key.pub /home/remote-user/.ssh/authorized_keys

# Set correct permissions for SSH key file
RUN chown remote-user:remote-user /home/remote-user/.ssh/authorized_keys && \
    chmod 600 /home/remote-user/.ssh/authorized_keys

# Generate SSH host keys
RUN ssh-keygen -A

# Install mysql Client
RUN apt-get install mysql-client -y

# Install AWS CLI v2 (official method)
RUN apt-get update && \
    apt-get install -y curl unzip && \
    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "/tmp/awscliv2.zip" && \
    unzip /tmp/awscliv2.zip -d /tmp && \
    /tmp/aws/install && \
    rm -rf /tmp/aws /tmp/awscliv2.zip && \
    rm -rf /var/lib/apt/lists/*

# Expose SSH port (optional, but good practice)
EXPOSE 22

# Start SSH daemon in foreground
CMD ["/usr/sbin/sshd", "-D"]
