# Copyright 2020 gRPC authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM php:8.0.0RC3-cli-buster

RUN apt-get -qq update && apt-get -qq install -y \
  autoconf automake git libtool pkg-config \
  valgrind wget zlib1g-dev

ARG MAKEFLAGS=-j8


WORKDIR /tmp

# install pear
RUN apt-get install expect -y && \
  curl -LO http://pear.php.net/go-pear.phar && \
  expect -c 'spawn php ./go-pear.phar; expect "or Enter to continue:"; send "\n"; expect "Currently used php.ini"; send "\n"; expect eof' && \
  rm go-pear.phar

WORKDIR /github/grpc

COPY . .

RUN pear package && \
  find . -name grpc-*.tgz | xargs -I{} pecl install {}


CMD php -d extension=grpc.so -r '$a = new \Grpc\Channel("dummy", []); echo get_class($a)."\n";'
