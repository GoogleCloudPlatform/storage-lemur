FROM centos:7
MAINTAINER Robert Read <robert.read@intel.com>

# Setup go build environment
RUN yum install -y @development golang pcre-devel glibc-static which

RUN mkdir -p /go/src /go/bin && chmod -R 777 /go
ENV GOPATH=/go \
    PATH=$GOPATH/bin:$PATH

RUN go get github.com/tools/godep && cp /go/bin/godep /usr/local/bin

ARG go_version
ARG go_macros_version

# Bootstrap a golang RPM build from Fedora Rawhide, but disable tests because they require privileged mode
RUN rpm -ivh http://mirrors.kernel.org/fedora/development/rawhide/Everything/x86_64/os/Packages/g/go-srpm-macros-${go_macros_version}.noarch.rpm \
	&& ln -s /usr/lib/rpm/macros.d/macros.go-srpm /etc/rpm/ \
	&& rpmbuild --define '%check exit 0' --rebuild http://mirrors.kernel.org/fedora/development/rawhide/Everything/source/tree/Packages/g/golang-${go_version}.src.rpm  \
	&& cd /root/rpmbuild/RPMS/x86_64 && rpm -Uvh golang-*.rpm ../noarch/golang-src-*.rpm
