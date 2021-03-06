PKGNAME=	librdkafka
LIBNAME=	librdkafka++
LIBVER=		1

CXXSRCS=	RdKafka.cpp ConfImpl.cpp HandleImpl.cpp \
		ConsumerImpl.cpp ProducerImpl.cpp KafkaConsumerImpl.cpp \
		TopicImpl.cpp TopicPartitionImpl.cpp MessageImpl.cpp \
		QueueImpl.cpp MetadataImpl.cpp

HDRS=		rdkafkacpp.h

OBJS=		$(CXXSRCS:%.cpp=%.o)



all: lib check


include ../mklove/Makefile.base

# No linker script/symbol hiding for C++ library
WITH_LDS=n

# OSX and Cygwin requires linking required libraries
ifeq ($(_UNAME_S),Darwin)
	FWD_LINKING_REQ=y
endif
ifeq ($(_UNAME_S),AIX)
	FWD_LINKING_REQ=y
endif
ifeq ($(shell uname -o 2>/dev/null),Cygwin)
	FWD_LINKING_REQ=y
endif

LIBS := -L../src -lrdkafka $(LIBS) 
LIBS += -lstdc++

CHECK_FILES+= $(LIBFILENAME) $(LIBNAME).a


file-check: lib
check: file-check

install: lib-install

clean: lib-clean

ifeq ($(WITH_LDS),y)
# Enable linker script if supported by platform
LIB_LDFLAGS+= $(LDFLAG_LINKERSCRIPT)$(LIBNAME).lds
endif

$(LIBNAME).lds: $(HDRS)
	@(printf "$(MKL_YELLOW)Generating linker script $@ from $(HDRS)$(MKL_CLR_RESET)\n" ; \
	  cat ../src/rdkafka.h | ../lds-gen.py > $@)

-include $(DEPS)
