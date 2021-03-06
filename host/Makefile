# Copyright (c) Microsoft Corporation. All rights reserved.
# Licensed under the MIT License.

# Detect C and C++ compiler options
# if not gcc and g++, default to clang-7
C_COMPILER=$(notdir $(CC))
ifeq ($(C_COMPILER), gcc)
        CXX_COMPILER=$(notdir $(CXX))
        USE_GCC = true
endif

ifeq ($(USE_GCC),)
        CC = clang-7
        CXX = clang++-7
        C_COMPILER=clang
        CXX_COMPILER=clang++
endif

CFLAGS=$(shell pkg-config oehost-$(C_COMPILER) --cflags)
CXXFLAGS=$(shell pkg-config oehost-$(CXX_COMPILER) --cflags)
LDFLAGS=$(shell pkg-config oehost-$(CXX_COMPILER) --libs)

all: build

build:
	@ echo "Compilers used: $(CC), $(CXX)"
	oeedger8r ../helloworld.edl --untrusted
	/usr/local/bin/protoc -I../ ../hello.proto --cpp_out=.
	$(CXX) -c  -g $(CXXFLAGS) -std=c++11 host.cpp
	$(CC) -c -g $(CFLAGS) helloworld_u.c
	$(CXX) -c -g $(CXXFLAGS) -std=c++11 hello.pb.cc
	$(CXX) -o helloworldhost host.o helloworld_u.o hello.pb.o /usr/local/lib/libprotobuf.a -Wl,--version-script=version.file  $(LDFLAGS) -Wl,--no-undefined

clean:
	rm -f file-encryptorhost fileencryptor_u.* fileencryptor_args.h *.o ../out.decrypted ../out.encrypted

