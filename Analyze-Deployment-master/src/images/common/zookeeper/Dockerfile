#/********************************************************************************
# * (C) Copyright IBM Corporation 2018, 2019.
# *
# * This program and the accompanying materials are made available under the
# * terms of the Eclipse Public License 2.0 which is available at
# * http://www.eclipse.org/legal/epl-2.0.
# *
# * SPDX-License-Identifier: EPL-2.0
# *
# ********************************************************************************/

FROM ubuntu_toolkit_image

RUN apt-get install -y \
    openssh-server

#The host name of the Docker container.
ENV hostname zookeeper

#The custom configuration overrides the base configuration in the ubuntu_toolkit_image.
COPY --chown=i2analyze:i2analyze configuration ${toolkitDir}/configuration

#Change the user that runs commands.
USER i2analyze:i2analyze

#Install ZooKeeper.
RUN /opt/IBM/i2analyze/toolkit/scripts/setup -t installZookeeper

#Create the ZooKeeper host and the configuration for the ZooKeeper hosts on this server.
RUN /opt/IBM/i2analyze/toolkit/scripts/setup -t createZkHosts --hostname ${hostname}

RUN echo "/opt/IBM/i2analyze/toolkit/scripts/setup -t createAndUploadSolrConfig --hostname ${hostname} || echo \"Error stating Zookeeper host in ${hostname}\"" >> /opt/IBM/startupExt

EXPOSE 9983 22

#Start ZooKeeper and upload the Solr configuration to ZooKeeper.
ENTRYPOINT /opt/IBM/startupExt && tail -f /dev/null
