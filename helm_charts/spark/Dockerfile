# Spark base image
FROM bitnami/spark:3.1.3
# run as root
USER root
# Create analytics user and set locale
RUN useradd -m analytics && echo "export LC_ALL=en_US.UTF-8" >> /home/analytics/.bashrc
#Install needed packages
RUN  apt-get update && apt-get install -y sudo openjdk-11-jdk curl rsync  zip python3.6 python3-pip libffi-dev libssl-dev build-essential lzop \
     gcc autoconf g++ gnupg automake bison libc6-dev libgdbm-dev libncurses5-dev libsqlite3-dev pkg-config sqlite3 zlib1g-dev libtool libyaml-dev make libgmp-dev libreadline-dev gpgv2 gem ruby gnupg2 virtualenv \
     && curl --silent --remote-name  https://downloads.lightbend.com/scala/2.12.10/scala-2.12.10.deb && dpkg -i scala-2.12.10.deb \
     && apt-get clean && rm -rf /var/lib/apt/lists/*
# Define the Spark JARs folder path and array of files 
ARG SPARK_JARS_FOLDER=/opt/bitnami/spark/jars \
    FILE_URLS="\
    https://repo1.maven.org/maven2/com/google/guava/guava/19.0/guava-19.0.jar \
    https://repo1.maven.org/maven2/net/java/dev/jets3t/jets3t/0.9.4/jets3t-0.9.4.jar \
    https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-aws/2.7.3/hadoop-aws-2.7.3.jar \
    https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-azure/2.7.3/hadoop-azure-2.7.3.jar \
    https://repo1.maven.org/maven2/com/microsoft/azure/azure-storage/3.0.0/azure-storage-3.0.0.jar \
    https://repo1.maven.org/maven2/com/jamesmurty/utils/java-xmlbuilder/1.1/java-xmlbuilder-1.1.jar" \
    DIRS="/home/analytics/sbin /mount/data/analytics /mount/data/analytics/logs /mount/data/analytics/logs/services /mount/data/analytics/logs/data-products /mount/data/analytics/logs/api-service /mount/data/analytics/api /mount/data/analytics/tmp /mount/data/analytics/scripts/monitor-data /mount/data/analytics/models-2.0"
# Set the Spark JARs folder as the default directory for downloading files
WORKDIR $SPARK_JARS_FOLDER

# remove old versions and Download files to the Spark JARs folder 
RUN rm guava-14.0.1.jar && \
    for url in $FILE_URLS; do curl --silent --remote-name $url ; done && \
# Create required directories and change ownership of mount dir 
    for dir in $DIRS ; do mkdir -p $dir ; done && \
    chown -R analytics:analytics /mount
