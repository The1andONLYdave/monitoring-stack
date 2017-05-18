#
# FROM https://hub.docker.com/r/uchiru/mtail/~/dockerfile/
#
FROM alpine:latest

RUN apk add --update musl go git bash make libc-dev musl-dev

ENV GOPATH     /go
ENV PATH       /go/bin:$PATH
ENV SRC_PATH   /go/src/github.com/google/mtail
ENV MTAIL_HASH dd1b8effd61821cab31aa7edbf17882e1243641c

RUN mkdir -p $SRC_PATH && cd $SRC_PATH/../ && \
	git clone https://github.com/google/mtail.git && \
	cd $SRC_PATH && git reset --hard $MTAIL_HASH

RUN cd $SRC_PATH && make install_coverage_deps
RUN cd $SRC_PATH && make install_gen_deps
RUN cd $SRC_PATH && make install
RUN mv `which mtail` /usr/local/bin/mtail
RUN rm -fr $GOPATH


COPY progs/* /progs/

EXPOSE 3903

ENTRYPOINT [ "/usr/local/bin/mtail", "-logtostderr",  "-progs","/progs", "-logs","/rootfs/var/log/messages"]