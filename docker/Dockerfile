FROM ubuntu:16.10

# needed for ./try-reflex
RUN apt-get update && apt-get install -y \
    bzip2 \
    curl \
    git \
    sudo

# conveniences
RUN apt-get update && apt-get install -y \
    net-tools \
    python \
    tmux \
    vim-gtk 

## Enable sudo w/o password
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

ENV U reflex

## Set up user "reflex" to run try-reflex
## Assign arbitrary uid to avoid collision with user on host machine
RUN adduser --disabled-password --gecos '' --uid 21348 $U 
RUN adduser $U sudo 

WORKDIR /home/$U/reflex-platform

ADD . ./
RUN chown -R $U .

# user level config required by tests
RUN docker/run-as-reflex.sh 'docker/setup-reflex-user.sh'

# Run try-reflex (takes a while the first time)
RUN docker/run-as-reflex.sh './try-reflex --run exit'

# Run commands as user reflex by default
# Tried docker "USER" here but that had problems - not sure why.
ENTRYPOINT ["docker/run-as-reflex.sh"]

# run try-reflex by default.
CMD ["./try-reflex"]

# Set the locale 
# Got this originally here: # http://askubuntu.com/questions/581458/how-to-configure-locales-to-unicode-in-a-docker-ubuntu-14-04-container
#
# Prevents some terminal related problems.
#
RUN locale-gen en_US.UTF-8  
ENV LANG en_US.UTF-8  
ENV LANGUAGE en_US:en  
ENV LC_ALL en_US.UTF-8  
