
FROM mcr.microsoft.com/dotnet/core/sdk:2.2 as core

FROM openjdk:8u212-jre


#IF you have a local copy of the spark gz then reference that as a COPY (which will automatically extract the files) it will
#be a million times faster than ADD from url then extract. Comment out these two lines and uncomment "ADD ./blah"

ADD https://www-us.apache.org/dist/spark/spark-2.4.3/spark-2.4.3-bin-hadoop2.7.tgz /tmp
RUN tar xf /tmp/spark-2.4.3-bin-hadoop2.7.tgz -C /usr/local/

#ADD ./spark-2.4.3-bin-hadoop2.7.tgz /usr/local

COPY --from=core /usr/share/dotnet /usr/share/dotnet
CMD "ln -s /usr/share/dotnet/dotnet"

RUN apt-get install \
        ca-certificates \        
        libssl1.0 \
        libstdc++ \
        tzdata 

ENV \
    # Enable detection of running in a container
    DOTNET_RUNNING_IN_CONTAINER=true \
    # Set the invariant mode since icu_libs isn't included (see https://github.com/dotnet/announcements/issues/20)
    DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=true

ENV SPARK_HOME=/usr/local/spark-2.4.3-bin-hadoop2.7
ENV PATH=:${PATH}:${SPARK_HOME}/bin:/usr/share/dotnet


RUN apt-get update
RUN apt-get install git --assume-yes

RUN dotnet --info
