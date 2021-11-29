# define build arg
ARG cpu_thread="15"
ARG build_dir="/build_space"

ARG gcc_version="10.3.0"
ARG gcc_install_prefix="/usr/latest_gcc"

ARG cmake_version="3.22.0"
ARG cmake_install_prefix="/usr/cmake"


# gcc build layer
FROM chatblanc/centos_minimal_gcc:latest AS gcc_build

SHELL ["/bin/bash", "-c"]
ARG cpu_thread
ARG build_dir
ARG gcc_version
ARG gcc_install_prefix

# download gcc source code
WORKDIR ${build_dir}
RUN wget http://ftp.tsukuba.wide.ad.jp/software/gcc/releases/gcc-${gcc_version}/gcc-${gcc_version}.tar.gz && \
	tar -xzf gcc-${gcc_version}.tar.gz && \
# download required gcc tool
	cd gcc-${gcc_version} && \
	./contrib/download_prerequisites && \
# configure gcc build
	mkdir build && \
	cd build && \
	../configure --prefix=${gcc_install_prefix} \
	--enable-languages=c,c++ \
	--disable-multilib
#	\ --program-suffix=-10.3

# gcc build using cpu threads and install gcc at ${gcc_install_prefix}
WORKDIR ${build_dir}/gcc-${gcc_version}/build
RUN mkdir ${gcc_install_prefix} && \
	make -j ${cpu_thread} && \
	make install -j ${cpu_thread}


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
ARG gcc_install_prefix
ARG cmake_install_prefix

#RUN mkdir ${gcc_install_prefix}
COPY --from=gcc_build ${gcc_install_prefix} ${gcc_install_prefix}
COPY --from=cmake_build ${cmake_install_prefix} ${cmake_install_prefix}

# define environment variables
ENV LATEST_GCC=${gcc_install_prefix}
ENV PATH=${gcc_install_prefix}/bin:$PATH
ENV PATH=${cmake_install_prefix}/bin:$PATH
ENV LD_LIBRARY_PATH=${gcc_install_prefix}/lib64:$LD_LIBRARY_PATH
ENV LD_RUN_PATH=${gcc_install_prefix}/lib64:$LD_RUN_PATH

WORKDIR /workspace

