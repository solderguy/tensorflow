# This Makefile compiles the label_image example for the Raspberry Pi.
# See tensorflow/contrib/pi_examples/README.md for full build instructions.

# Find where we're running from, so we can store generated files here.
SCRIPT_DIR := $(shell dirname $(realpath $(lastword $(MAKEFILE_LIST))))

# The location of the tensorflow/contrib/makefile directory.
TFMAKEFILE_DIR := $(SCRIPT_DIR)/../../makefile

# Where compiled objects are stored.
GENDIR := $(SCRIPT_DIR)/gen/
OBJDIR := $(GENDIR)obj/
LIBDIR := $(GENDIR)lib/
BINDIR := $(GENDIR)bin/

# The expected locations of the TensorFlow library.
TFLIBDIR := $(TFMAKEFILE_DIR)/gen/lib
TFLIBS := $(TFLIBDIR)/libtensorflow-core.a

# Where the downloads have been stored.
DOWNLOADSDIR := $(TFMAKEFILE_DIR)/downloads

# The location of the compiled protobuf headers generated by TensorFlow.
PBTGENDIR := $(TFMAKEFILE_DIR)/gen/proto_text/
PROTOGENDIR := $(TFMAKEFILE_DIR)/gen/proto/

# The name of the output program we're compiling.
EXECUTABLE_NAME := $(BINDIR)/label_image

# Settings for the target compiler.
CXX := gcc
OPTFLAGS := -O0
CXXFLAGS := --std=c++11 $(OPTFLAGS)
LDFLAGS := \
-L/usr/local/lib \
-L$(TFLIBDIR) \
-Wl,--no-whole-archive
INCLUDES := \
-I/usr/local/include \
-I. \
-I$(DOWNLOADSDIR) \
-I$(DOWNLOADSDIR)/eigen-latest/ \
-I$(PROTOGENDIR) \
-I$(PBTGENDIR)
LIBS := \
-lstdc++ \
-lprotobuf \
-Wl,--allow-multiple-definition \
-Wl,--whole-archive \
-ltensorflow-core \
-Wl,--no-whole-archive \
-ldl \
-lpthread \
-lm \
-ljpeg
LIBFLAGS :=

EXECUTABLE_SRCS := tensorflow/contrib/pi_examples/label_image/label_image.cc

# File names of the intermediate files target compilation generates.
EXECUTABLE_OBJS := $(addprefix $(OBJDIR), $(EXECUTABLE_SRCS:.cc=.o))

.PHONY: clean

# The target that's compiled if there's no command-line arguments.
all: $(EXECUTABLE_NAME)

# Rules for target compilation.

$(EXECUTABLE_NAME): $(EXECUTABLE_OBJS) $(TFLIBS)
	@mkdir -p $(dir $@)
	$(CXX) $(CXXFLAGS) $(INCLUDES) \
	-o $(EXECUTABLE_NAME) $(EXECUTABLE_OBJS) \
	$(LIBFLAGS) $(LIB_PATH) $(LDFLAGS) $(LIBS)

# Matches on C++ source files.
$(OBJDIR)%.o: %.cc 
	@mkdir -p $(dir $@)
	$(CXX) $(CXXFLAGS) $(INCLUDES) -c $< -o $@

# Gets rid of all generated files.
clean:
	rm -rf $(GENDIR)
