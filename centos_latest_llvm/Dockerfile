# define build arg
ARG cpu_thread="15"
ARG build_dir="/build_space"

ARG gcc_install_prefix="/usr/latest_gcc"
ARG cmake_install_prefix="/usr/cmake"
ARG llvm_install_prefix="/usr/llvm"

ARG llvm_repo_tag="llvmorg-13.0.0"

# llvm build layer
FROM chatblanc/centos_latest_gcc:latest AS llvm_build

SHELL ["/bin/bash", "-c"]
ARG cpu_thread
ARG build_dir
ARG gcc_install_prefix
ARG cmake_install_prefix
ARG llvm_install_prefix
ARG llvm_repo_tag

# install packages
RUN yum install -y \
	git \
	python3 \
	ld-linux.so.2

# LLVM clone from git
WORKDIR ${build_dir}
RUN git clone https://github.com/llvm/llvm-project.git -b ${llvm_repo_tag} && \
	mkdir -p ${llvm_install_prefix}

# llvm build using cpu threads and install llvm at ${llvm_install_prefix}
	#define CC & CXX(enable to write full path)
RUN cd llvm-project && \
	mkdir build && cd build && \
	CC=${gcc_install_prefix}/bin/gcc \
	CXX=${gcc_install_prefix}/bin/g++ \
	cmake \
		-G "Unix Makefiles" \
		-DCMAKE_INSTALL_PREFIX=${llvm_install_prefix} \
		-DGCC_INSTALL_PREFIX=${gcc_install_prefix} \
		-DCMAKE_BUILD_TYPE=Release \
		-DLLVM_ENABLE_ASSERTIONS=On \
		-DLLVM_ENABLE_PROJECTS="clang;clang-tools-extra;compiler-rt;libunwind;parallel-libs" \
		../llvm && \
	make -j ${cpu_thread} && \
	make install -j ${cpu_thread}



# final layer
FROM chatblanc/centos_latest_gcc:latest

SHELL ["/bin/bash", "-c"]
ARG llvm_install_prefix

# install packages
RUN yum install -y \
	git \
	python3 \
	ld-linux.so.2

COPY --from=llvm_build ${llvm_install_prefix} ${llvm_install_prefix}

# define environment variables
ENV LATEST_LLVM=${llvm_install_prefix}
ENV PATH=${llvm_install_prefix}/bin:$PATH


WORKDIR /workspace

