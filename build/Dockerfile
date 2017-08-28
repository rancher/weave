FROM zambon/golang-multi:1.8.3

ARG RACE

RUN apt-get -q update && \
    apt-get -qy install libpcap-dev python-requests time file && \
    rm -rf /var/lib/apt/lists/*

# Get latest versions at the time of the original build (Nov 4, 2016, 6:30 PM EDT)
# https://github.com/rancher/weave/commit/00a22c8f6697ecd32669b223e7af59ebc2cb1014
# Changes on top of tag v1.7.2 (ebfaf11f4a4058d7858b6881541626a7ce0d9166)
# https://github.com/weaveworks/weave/compare/ebfaf11f4a4058d7858b6881541626a7ce0d9166...rancher:check-routes
RUN set -x && \
  mkdir -p $GOPATH/src/github.com/golang && \
  cd $GOPATH/src/github.com/golang && \
  git clone https://github.com/golang/lint.git && \
  cd lint && \
  git checkout 3390df4df2787994aea98de825b964ac7944b817 && \
  \
  mkdir -p $GOPATH/src/github.com/fzipp && \
  cd $GOPATH/src/github.com/fzipp && \
  git clone https://github.com/fzipp/gocyclo.git && \
  cd gocyclo && \
  git checkout 6acd4345c835499920e8426c7e4e8d7a34f1bb83 && \
  \
  mkdir -p $GOPATH/src/github.com/client9 && \
  cd $GOPATH/src/github.com/client9 && \
  git clone https://github.com/client9/misspell.git && \
  cd misspell && \
  git checkout 5cb2f5365e6061dc8e8f4bd6b4aadf553f1a015f

RUN go get github.com/golang/lint/golint github.com/fzipp/gocyclo github.com/client9/misspell/cmd/misspell
RUN go clean -i net && go install -tags netgo std
RUN go install ${RACE} -tags netgo std
COPY build.sh /
ENTRYPOINT ["sh", "/build.sh"]
