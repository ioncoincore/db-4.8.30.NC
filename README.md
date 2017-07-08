Berkeley DB
-----------
It is recommended to use Berkeley DB 4.8. If you have to build it yourself:

BerkeleyDB is required for the [🗺️Ion Digital Currency ©️ 👯👯 👛 (by 🐼CEVAP🐼)](https://github.com/cevap/ion/).

**For Ubuntu only:** db4.8 packages are available [here](https://launchpad.net/~ionomy/+archive/ubuntu/ioncoin/).
You can add the repository and install using the following commands:

    sudo apt-get install software-properties-common
    sudo add-apt-repository ppa:ionomy/ioncoin
    sudo apt-get update
    sudo apt-get install libdb4.8-dev libdb4.8++-dev

### Script with download from CEVAP github
```bash
ION_ROOT=$(pwd)

# Pick some path to install BDB to, here we create a directory within the ion directory
BDB_PREFIX="${ION_ROOT}/db4"
mkdir -p $BDB_PREFIX

# Fetch the source and verify that it is not tampered with
git clone https://github.com/cevap/db-4.8.30.NC.git

# Build the library and install to our prefix
cd db-4.8.30.NC/build_unix/
#  Note: Do a static build so that it can be embedded into the executable, instead of having to find a .so at runtime
../dist/configure --enable-cxx --disable-shared --with-pic --prefix=$BDB_PREFIX
make install

# Configure Ion Core to use our own-built instance of BDB
cd $ION_ROOT
./autogen.sh
./configure LDFLAGS="-L${BDB_PREFIX}/lib/" CPPFLAGS="-I${BDB_PREFIX}/include/" # (other args...)
```

### Script with download from oracle directly
```bash
ION_ROOT=$(pwd)

# Pick some path to install BDB to, here we create a directory within the ion directory
BDB_PREFIX="${ION_ROOT}/db4"
mkdir -p $BDB_PREFIX

# Fetch the source and verify that it is not tampered with
wget 'http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz'
echo '12edc0df75bf9abd7f82f821795bcee50f42cb2e5f76a6a281b85732798364ef  db-4.8.30.NC.tar.gz' | sha256sum -c
# -> db-4.8.30.NC.tar.gz: OK
tar -xzvf db-4.8.30.NC.tar.gz

# Build the library and install to our prefix
cd db-4.8.30.NC/build_unix/
#  Note: Do a static build so that it can be embedded into the executable, instead of having to find a .so at runtime
../dist/configure --enable-cxx --disable-shared --with-pic --prefix=$BDB_PREFIX
make install

# Configure Ion Core to use our own-built instance of BDB
cd $ION_ROOT
./autogen.sh
./configure LDFLAGS="-L${BDB_PREFIX}/lib/" CPPFLAGS="-I${BDB_PREFIX}/include/" # (other args...)
```

**Note**: You only need Berkeley DB if the wallet is enabled (see the section *Disable-Wallet mode* below).
