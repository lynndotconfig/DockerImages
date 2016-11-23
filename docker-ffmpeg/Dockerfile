FROM kunkka/centos:7
MAINTAINER lipingwang <lipingwang@gstarcloud.com>

# get dependencies
RUN yum -y update

# add rpm repo
RUN yum -y install epel-release
RUN rpm --import http://li.nux.ro/download/nux/RPM-GPG-KEY-nux.ro
RUN rpm -Uvh http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-1.el7.nux.noarch.rpm

# install ffmpeg
RUN yum -y install ffmpeg ffmpeg-devel
