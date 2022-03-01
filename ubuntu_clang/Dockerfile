FROM ubuntu:latest

ENV TZ=Asia/Tokyo
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

SHELL ["/bin/bash", "-c"]

# install packages
RUN apt-get update -y && \
	apt-get install -y \
	git \
	make \
	cmake \
	build-essential \
	clang \
	clang-format \
	libeigen3-dev
