# Bunnycoin [BUNN ,BUN]

Bunnycoin is a decentralized peer-to-peer cryptocurrency based upon Bitcoin (inspired by Dogecoin), designed to spread Love, Peace, Happiness and Economic Freedom Worldwide.

### How many bunny can exist?

Approximately 100 Billion Bunnies in total. 10% of coins mined go to support user voted upon charities and good will causes using the revolutionary built-in decentralized voting/distribution system.

### Burrowing for Bunnies?
Scrypt Proof of Work

1 Minute Block Targets, 4 Hour Diff Readjustments

Random block rewards to encourage individuals to participate in the mining process.

1-100,000: 0-1,000,000 Bunnycoin Reward

100,001 - 200,000: 0-500,000 Bunnycoin Reward(random)

200,001 - 300,000: 0-250,000 Bunnycoin Reward(random)

300,001 - 400,000: 0-125,000 Bunnycoin Reward(random)

400,001 - 500,000: 0-62,500 Bunnycoin Reward(random)

500,001 - 600,000: 0-31,250 Bunnycoin Reward(random)

600,000+ : 0-10,000 Bunnycoin Reward (random)


### Ports
RPC 48444

P2P 48445

### Difference between original(1.0.0) and this branch(1.0.0a)

Support     : ubuntu 16.04 or later

unsupported : ubuntu 14.04/12.04

- Versions used in this release:
-  GCC           5.4.0
-  OpenSSL       1.0.2g
-  Berkeley DB   4.8.30.NC
-  Boost         1.58
-  miniupnpc     1.9

The default BerkeleyDB on Ubuntu 16.04 is Ver 5.1.

BerkeleyDB 4.8 and 5.1 are incompatible.
So,you must install BerkeleyDB 4.8 manually.

### How to install 1.0.0a to ubuntu 16.04

Build requirements:

	sudo apt-get install build-essential
  sudo apt-get install libssl-dev
  sudo apt-get install libboost-all-dev

 db4.8 packages are available [here](https://launchpad.net/~bitcoin/+archive/bitcoin).

 Ubuntu precise has packages for libdb5.1-dev and libdb5.1++-dev,
 but using these will break binary wallet compatibility, and is not recommended.

  sudo apt-get install libdb4.8-dev
  sudo apt-get install libdb4.8++-dev

  Optional:

  sudo apt-get install libminiupnpc-dev (see USE_UPNP compile flag)

Berkeley DB:
You need Berkeley DB 4.8.  If you have to build Berkeley DB yourself:

  wget http://download.oracle.com/berkeley-db/db-4.8.30.tar.gz
  tar zxvf db-4.8.30.tar.gz
  cd db-4.8.30.tar/build_unix/    
  ../dist/configure --prefix=/usr/local/BerkeleyDB.4.8 --enable-compat185 --enable-cxx
  sudo make
  sudo make install

To Build

  cd src/
  make -f makefile.unix		# Headless bunnycoin

See readme-qt.rst for instructions on building Bunnycoin-Qt, the graphical user interface.
