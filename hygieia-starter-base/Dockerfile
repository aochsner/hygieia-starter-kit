# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM centos/systemd

ENV container docker

COPY mongodb-org.repo /etc/yum.repos.d/

RUN mkdir -p /hygieia/logs; \
mkdir -p /hygieia/ui/; \
mkdir -p /hygieia/api/; \
mkdir -p /hygieia/collectors/; \
mkdir -p /hygieia/cacerts

RUN  yum -y install mongodb-org; \
yum -y install httpd; \
yum -y install java-1.8.0-openjdk-devel; \
yum clean all; 

# Pulls API jar
RUN curl -o /hygieia/api/api.jar http://central.maven.org/maven2/com/capitalone/dashboard/api/3.1.1/api-3.1.1.jar

# Pulls UI tar
RUN curl -o /hygieia/ui/ui.tar.gz http://central.maven.org/maven2/com/capitalone/dashboard/UI/3.0.2/UI-3.0.2-src.tar.gz

# Pulls Github Collector jar
RUN curl -o /hygieia/collectors/github-scm-collector.jar http://central.maven.org/maven2/com/capitalone/dashboard/github-scm-collector/3.1.0/github-scm-collector-3.1.0.jar

# Pulls Sonar Collector jar
RUN curl -o /hygieia/collectors/sonar-codequality-collector.jar http://central.maven.org/maven2/com/capitalone/dashboard/sonar-codequality-collector/3.1.0/sonar-codequality-collector-3.1.0.jar

# Pulls Jenkins Collector jar
RUN curl -o /hygieia/collectors/jenkins-build-collector.jar http://central.maven.org/maven2/com/capitalone/dashboard/jenkins-build-collector/3.1.0/jenkins-build-collector-3.1.0.jar

# Bind to 0.0.0.0 to allow for external connection 
RUN sed -i "s|bindIp: 127.0.0.1|bindIp: 0.0.0.0|g" /etc/mongod.conf

WORKDIR /hygieia/ui
RUN tar -zvxf ui.tar.gz
RUN mv UI-3.0.2/dist/* /var/www/html/

CMD ["/usr/sbin/init"]