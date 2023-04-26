
BASEDIR = $(shell pwd)

compile: snappy

snappy: system/lib/libsnappy.a

system/lib/libsnappy.a:
	(cd $(REBAR_DEPS_DIR)/snappy/c_src && \
	mkdir -p build && cd build && \
	mkdir -p $(BASEDIR)/system && \
	cmake -D SNAPPY_BUILD_TESTS=0 -D SNAPPY_BUILD_BENCHMARKS=0 \
	      -D CMAKE_INSTALL_PREFIX=$(BASEDIR)/system \
	      ../ && $(MAKE) && $(MAKE) install)
	mv system/lib64 system/lib || true

clean:
	rm -rf system $(REBAR_DEPS_DIR)/snappy/c_src/snappy/build
