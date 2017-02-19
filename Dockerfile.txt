FROM ubuntu:16.04

RUN apt-get update
RUN apt-get install firefox -y
RUN apt-get install libgl1-mesa-glx -y

RUN apt-get install git -y
RUN git config --global user.name "Catalin Lazar"
RUN git config --global user.email "lazcatluc@gmail.com"
RUN git config --global push.default simple

RUN apt-get install curl
RUN curl -sL https://deb.nodesource.com/setup_6.x | bash - 
RUN apt-get install -y nodejs

RUN sed 's/main$/main universe/' -i /etc/apt/sources.list && \
    apt-get update && apt-get install -y software-properties-common && \
    add-apt-repository ppa:webupd8team/java -y && \
    apt-get update && \
    echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections && \
    apt-get install -y oracle-java8-installer libxext-dev libxrender-dev libxtst-dev && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /tmp/*

RUN apt-get update && apt-get install -y libgtk2.0-0 libcanberra-gtk-module

RUN echo 'Installing eclipse'
RUN wget http://download.springsource.com/release/ECLIPSE/neon/2/eclipse-jee-neon-2-linux-gtk-x86_64.tar.gz -O eclipse.tar.gz
RUN tar -xf eclipse.tar.gz -C /opt
RUN rm eclipse.tar.gz
RUN sed -i 's/1024m/2048m/g' /opt/eclipse/eclipse.ini
RUN /opt/eclipse/eclipse -application org.eclipse.equinox.p2.director -noSplash -repository http://www.nodeclipse.org/updates/ -installIUs org.nodeclipse.feature.group,org.nodeclipse.common,org.nodeclipse.pluginslist.feature.feature.group,org.nodeclipse.enide.editors.jade.feature.feature.group,org.nodeclipse.mongodb.feature.feature.group,org.nodeclipse.phantomjs.feature.feature.group
RUN /opt/eclipse/eclipse -application org.eclipse.equinox.p2.director -noSplash -repository http://github.eclipsesource.com/jshint-eclipse/updates -installIUs com.eclipsesource.jshint.feature.feature.group
RUN /opt/eclipse/eclipse -application org.eclipse.equinox.p2.director -noSplash -repository http://repo1.maven.org/maven2/.m2e/connectors/m2eclipse-egit/0.14.0/N/LATEST/ -installIUs org.sonatype.m2e.egit.feature.feature.group
RUN /opt/eclipse/eclipse -application org.eclipse.equinox.p2.director -noSplash -repository http://update.eclemma.org/ -installIUs com.mountainminds.eclemma.feature.feature.group
RUN mkdir -p /root/workspace/.metadata/.plugins/org.eclipse.core.runtime
RUN git clone https://github.com/lazcatluc/eclipse-settings /root/workspace/.metadata/.plugins/org.eclipse.core.runtime/.settings

CMD /opt/eclipse/eclipse -data /root/workspace && echo 'Pausing to allow save state' && /bin/bash