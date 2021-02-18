## Microsoft SEAL For Python

Microsoft [**SEAL**](https://github.com/microsoft/SEAL) is an easy-to-use open-source ([MIT licensed](https://github.com/microsoft/SEAL/blob/master/LICENSE)) homomorphic encryption library developed by the Cryptography Research group at Microsoft.

[**pybind11**](https://github.com/pybind/pybind11) is a lightweight header-only library that exposes C++ types in Python and vice versa, mainly to create Python bindings of existing C++ code.

This is a python binding for the Microsoft SEAL library.



## Contents

* [Build](#build)
* [FAQ](#faq)
* [Note](#note)
  * [Serialize](#serialize)
  * [Other](#other)



## Build
### Linux
CMake (>= 3.10), GNU G++ (>= 6.0) or Clang++ (>= 5.0), Python (>=3.6.8)

```shell
# Optional
sudo apt-get install build-essential cmake python3 python3-dev python3-pip
# Numpy is essential
pip3 install -r requirements.txt

# Get the repo or download from the releases
git clone https://github.com/Huelse/SEAL-Python.git
git submodule init

# Build the SEAL lib
cd ./SEAL
cmake -S . -B build -DSEAL_USE_MSGSL=OFF -DSEAL_USE_ZLIB=OFF -DSEAL_USE_ZSTD=OFF
cmake --build build
cd ..

# Run the setup.py
python3 setup.py build_ext -i
```

### Windows

Visual Studio 2019 or newer is required. And use the **x64 Native Tools Command Prompt for Visual Studio 2019**  command prompt to configure and build the Microsoft SEAL library. It's usually can be found in your Start Menu.

```shell
# Same as above

# Build the SEAL lib
cmake -S . -B build -G "Visual Studio 16 2019" -A x64 -DSEAL_USE_MSGSL=OFF -DSEAL_USE_ZLIB=OFF -DSEAL_USE_ZSTD=OFF
cmake --build build --config Release

# Run the setup.py
python setup.py build_ext -i
```

Microsoft official video [SEAL in windows](https://www.microsoft.com/en-us/research/video/installing-microsoft-seal-on-windows/).



## FAQ

1. ImportError: undefined symbol

   Build a shared SEAL library `cmake . -DBUILD_SHARED_LIBS=ON`, and get the `libseal.so`,

   then change the path in `setup.py`, and rebuild.

2. ImportError: libseal.so... cannot find

   a. `sudo ln -s /path/to/libseal.so  /usr/lib`

   b. add `/usr/local/lib` or the `SEAL/native/lib` to `/etc/ld.so.conf` and refresh it `sudo ldconfig`

   c. build in cmake.
   
3. BuildError: C++17 at least

4. ModuleNotFoundError: No module named 'seal'

   The `.so` or `.pyd` file must be in the current directory, or you have `install` it already.



## Note

* **Serialize**

  In usually, you can use the SEAL's native serialize API to save the data.  Here is an example:

  ```python
  cipher.save('cipher')
  
  load_cipher = Ciphertext()
  load_cipher.load(context, 'cipher')  # work if the context is valid.
  ```

  Support type: `Encryptionparams, Ciphertext, Plaintext, SecretKey, Publickey, Relinkeys, Galoiskeys`

  Particularly, if you want to use the pickle to serialize your data, you need to do these things like below:

  ```shell
  # 1. Modify the serializable object's header file in SEAL and switch the wrapper.
  python helper.py
  
  # 2. Rebuild the SEAL lib like above
  cmake --build build
  
  # 3. Run the setup.py
  python setup.py build_ext -i
  ```

  Then, you can pickle the data object like this:

  ```python
  import pickle
  
  cipher.set_parms(parms)  # nccessary
  cipher_dump = pickle.dumps(cipher)
  cipher_load = pickle.loads(cipher_dump)
  ```
  
  Generally, we don't use the compress lib.
  
* **Other**

  There are a lot of changes in the latest SEAL lib, we try to make the API in python can be used easier, it may remain some problems we unknown, if any problems(bugs), [Issue](https://github.com/Huelse/SEAL-Python/issues) please.

  Email: [huelse@oini.top](mailto:huelse@oini.top?subject=Github-SEAL-Python-Issues)



## Contributing
* Professor: [Dr. Chen](https://zhigang-chen.github.io/)

* [Contributors](https://github.com/Huelse/SEAL-Python/graphs/contributors)

