# reproduct compile error - armv7

cf. PR https://github.com/Tracktion/tracktion_engine/issues/252

to reproduce the compilation error:

```bash
# 1. clone repository and sub-modules
git clone -b reproduce_armv7_compilation_failure https://github.com/iamdey/tracktion_engine.git
git submodule update --init --recursive

# 3. patch juce_dsp and crill an tracktion_CPU to support armv7

(cd modules/juce && patch -i ../../patches/juce_dsp.h.patch -p1)
(patch -i patches/crill-21.patch -p1)
(patch -i patches/tracktion_CPU.patch -p1)

# 4. try to compile with cross-compiler and its toolchain (cf. bellow)
docker run  --rm -v $PWD:/source iamdey/lmn-3-daw_compiler:armv7 /bin/bash -c '
		cd /source
		cmake -B build-armv7 -DCMAKE_TOOLCHAIN_FILE=/toolchain/toolchain.cmake
		cmake --build build-armv7 -j8
		'
```
