# Valgrind Exercise

## Memory Bug and Fix

### 1. Memory Leak in `AnalogSensor.cpp`

- **Buggy code:**
  ```cpp
  std::vector<int> *readings = new std::vector<int>(mSamples, 10);
  double result = std::accumulate( readings->begin(), readings->end(), 0.0 ) / readings->size();
  return result;
  ```
  This code allocates a vector on the heap with `new` but never deletes it, causing a memory leak.

- **Fixed code:**
  ```cpp
  std::vector<int> readings(mSamples, 10);
  double result = std::accumulate(readings.begin(), readings.end(), 0.0) / readings.size();
  return result;
  ```
  The vector is now allocated on the stack, so there is no leak.

### 2. Uninitialized Variable in `main.cpp`

- **Buggy code:**
  ```cpp
  bool terminator;
  if( terminator )
  {
      std::cout << "DONE" << std::endl;
  }
  ```
  The variable `terminator` is uninitialized, leading to undefined behavior.

- **Fixed code:**
  ```cpp
  bool terminator{true};
  if( terminator )
  {
      std::cout << "DONE" << std::endl;
  }
  ```
  The variable is now properly initialized.

## Standard install via command-line
```bash
# Configure the project and generate a native build system:
  # Must re-run this command whenever any CMakeLists.txt file has been changed.
  cmake -S ./ -B build/
# To build with debugging information, do:
  cmake -S ./ -B build/ -D CMAKE_BUILD_TYPE=Debug
# Compile and build the project:
  # rebuild only files that are modified since the last build
  cmake --build build/
  # or rebuild everything from scracth
  cmake --build build/ --clean-first
  # to see verbose output, do:
  cmake --build build/ --verbose
# Run program:
  ./build/app/shell-app
  # Run with Valgrind
  valgrind --leak-check=yes --track-origins=yes ./build/app/shell-app
# Clean
  cmake --build build/ --target clean
# Clean and start over:
  rm -rf build/
```

## Answers to Valgring exercise (Optional Extra Credit)

### 1. What happens when the executable is linked statically?  Does Valgrind still detect those same bugs?

If the executable is linked statically Valgrind does not detect the bugs.

### 2. Why or why not.

When dynamically linked, Valgrind can get into the standard memory management routines. When dynamically linked, this interception is limited. 
