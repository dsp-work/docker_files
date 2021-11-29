# define build arg
ARG cpu_thread="15"
ARG build_dir="/build_space"

ARG cmake_version="3.22.0"
ARG cmake_install_prefix="/usr/cmake"



# cmake install layer
FROM chatblanc/centos_minimal_gcc:latest AS cmake_build

SHELL ["/bin/bash", "-c"]
ARG build_dir
ARG cmake_version
ARG cmake_install_prefix

# download cmake binaries
WORKDIR ${build_dir}
RUN wget https://github.com/Kitware/CMake/releases/download/v${cmake_version}/cmake-${cmake_version}-linux-x86_64.tar.gz && \
	tar -xzf cmake-${cmake_version}-linux-x86_64.tar.gz && \
	cd cmake-${cmake_version}-linux-x86_64 && \
	mkdir ${cmake_install_prefix} && \
	cp -afr * ${cmake_install_prefix}



# final layer
FROM chatblanc/centos_minimal_gcc:latest

SHELL ["/bin/bash", "-c"]
ARG cmake_install_prefix

#RUN mkdir ${gcc_install_prefix}
COPY --from=cmake_build ${cmake_install_prefix} ${cmake_install_prefix}

# define environment variables
ENV PATH=${cmake_install_prefix}/bin:$PATH

WORKDIR /workspace

