# GDB

- Add debug symbol and disable optimization so the debugger sees unmangled code
```
$ g++ -g -O0 -o crash_test crash_test.cpp
```

- with CMake
```
target_compile_options(crash PRIVATE -g -O0)
```

```
$ sudo sysctl -w kernel.core_pattern="/tmp/core.%e.%p"  # pipe core dump in into /tmp/ with format of core.
$ ulimit -c unlimited  # enable core dumps in your shell
$ ./<your_executable>
$ gdb ./<your_executable> /tmp/core.crash_test.*

bt/up/down inside gdb
```
