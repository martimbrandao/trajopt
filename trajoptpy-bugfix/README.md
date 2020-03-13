# Bugfix

If you get an error "dynamic module does not define init function (initctrajoptpy)" when importing ctrajoptpy in python, try this:

```
mkdir build
cd build
cmake ..
make
cp ctrajoptpy.so ../../build/lib/
```
