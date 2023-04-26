# rebar3_external_example

Just an example about how to use [rebar3_external](https://github.com/joaohf/rebar3_external) to
fetch external dependencies.

## Running a session example

Preparing the example:

```
git clone https://github.com/joaohf/rebar3_external_example
mkdir _checkouts
cd _checkouts
git clone https://github.com/joaohf/rebar3_external
cd ..
```

Getting the deps (optional):

```
$ rebar3 get-deps
===> Analyzing applications...
===> Compiling rebar3_external
===> Analyzing applications...
===> Compiling rebar3_external
===> Verifying dependencies...
===> Fetching eleveldb (from {external,{git,"https://github.com/basho/leveldb",
                              {ref,"6fb82424b0a00f21ec2a25ec5fdf94e9a9700793"}},
                         "c_src",
                         [{description,"leveldb: A key-value store"},
                          {vsn,"2.0.38"}]})
===> Fetching snappy (from {external,{http,"https://github.com/google/snappy/archive/refs/tags/1.1.9.tar.gz",
                             "213b6324b7790d25f5368629540a172c"},
                       "c_src",
                       [{description,"Google's Snappy compression library"},
                        {vsn,"1.1.9"}]})
```

Start the build. At this point the project's Makefile is going to be called. Here is were a [handcraft Makefile](https://github.com/joaohf/rebar3_external_example/c_src/Makefile) is the key point to build the fetched external dependency:

```
$ rebar3 compile
===> Analyzing applications...                                
===> Compiling rebar3_external                  
===> Analyzing applications...                                                                         
===> Compiling rebar3_external                                                                         
===> Verifying dependencies...                 
make: Entering directory '/home/joaohf/work/opensource/rebar3_external_example/c_src'
(cd /home/joaohf/work/opensource/rebar3_external_example/_build/default/lib/snappy/c_src && \          
mkdir -p build && cd build && \
mkdir -p /home/joaohf/work/opensource/rebar3_external_example/c_src/system && \
cmake -D SNAPPY_BUILD_TESTS=0 -D SNAPPY_BUILD_BENCHMARKS=0 \
      -D CMAKE_INSTALL_PREFIX=/home/joaohf/work/opensource/rebar3_external_example/c_src/system \
      ../ && make && make install)
-- The C compiler identification is GNU 11.3.0
-- The CXX compiler identification is GNU 11.3.0

... [build output] ...

Install the project...
-- Install configuration: ""
-- Installing: /home/joaohf/work/opensource/rebar3_external_example/c_src/system/lib/libsnappy.a
-- Installing: /home/joaohf/work/opensource/rebar3_external_example/c_src/system/include/snappy-c.h
-- Installing: /home/joaohf/work/opensource/rebar3_external_example/c_src/system/include/snappy-sinksource.h
-- Installing: /home/joaohf/work/opensource/rebar3_external_example/c_src/system/include/snappy.h
-- Installing: /home/joaohf/work/opensource/rebar3_external_example/c_src/system/include/snappy-stubs-public.h
-- Installing: /home/joaohf/work/opensource/rebar3_external_example/c_src/system/lib/cmake/Snappy/SnappyTargets.cmake
-- Installing: /home/joaohf/work/opensource/rebar3_external_example/c_src/system/lib/cmake/Snappy/SnappyTargets-noconfig.cmake
-- Installing: /home/joaohf/work/opensource/rebar3_external_example/c_src/system/lib/cmake/Snappy/SnappyConfig.cmake
-- Installing: /home/joaohf/work/opensource/rebar3_external_example/c_src/system/lib/cmake/Snappy/SnappyConfigVersion.cmake
make[1]: Leaving directory '/home/joaohf/work/opensource/rebar3_external_example/_build/default/lib/snappy/c_src/build'
mv system/lib64 system/lib || true
mv: cannot stat 'system/lib64': No such file or directory
make: Leaving directory '/home/joaohf/work/opensource/rebar3_external_example/c_src'
===> Analyzing applications...
===> Compiling rebar3_external_example
```

Clean also works as expected:

```
$ rebar3 clean
===> Analyzing applications...
===> Compiling rebar3_external
===> Analyzing applications...
===> Compiling rebar3_external
===> Verifying dependencies...
===> Cleaning out rebar3_external_example...
make: Entering directory '/home/joaohf/work/opensource/rebar3_external_example/c_src'
rm -rf system /home/joaohf/work/opensource/rebar3_external_example/_build/default/lib/snappy/c_src/snappy/build
make: Leaving directory '/home/joaohf/work/opensource/rebar3_external_example/c_src'
make: Entering directory '/home/joaohf/work/opensource/rebar3_external_example/c_src'
rm -rf system /home/joaohf/work/opensource/rebar3_external_example/_build/default/lib/snappy/c_src/snappy/build
make: Leaving directory '/home/joaohf/work/opensource/rebar3_external_example/c_src'
```

Tree command showing the external dependencies:

```
$ rebar3 tree
===> Analyzing applications...
===> Compiling rebar3_external
===> Analyzing applications...
===> Compiling rebar3_external
===> Verifying dependencies...
└─ rebar3_external_example─0.1.0 (project app)
   ├─ eleveldb─2.0.38 (external repo)
   └─ snappy─1.1.9 (external repo)
```

Checking rebar.lock:

```
[{<<"eleveldb">>,
  {external,{git,"https://github.com/basho/leveldb",
                 {ref,"6fb82424b0a00f21ec2a25ec5fdf94e9a9700793"}},
            "c_src",
            [{description,"leveldb: A key-value store"},{vsn,"2.0.38"}]},
  0},
 {<<"snappy">>,
  {external,{http,"https://github.com/google/snappy/archive/refs/tags/1.1.9.tar.gz",
                  "213b6324b7790d25f5368629540a172c"},
            "c_src",
            [{description,"Google's Snappy compression library"},
             {vsn,"1.1.9"}]},
  0}].
```
    $ rebar3 get-deps
```

```
    $ rebar3 compile
```

```
    $ rebar3 clean
```

```
    $ rebar3 tree
```
