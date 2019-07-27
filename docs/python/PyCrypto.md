PyCrypto 
========

version: 2.7.0

License: Public Domain 

http://www.pycrypto.org/ 
https://www.dlitz.net/software/pycrypto/
https://pypi.org/project/pycrypto/


usage
======

An example usage of the SHA256 module is:
>>> from Crypto.Hash import SHA256
>>> hash = SHA256.new()
>>> hash.update('message')
>>> hash.digest()
'\xabS\n\x13\xe4Y\x14\x98+y\xf9\xb7\xe3\xfb\xa9\x94\xcf\xd1\xf3\xfb"\xf7\x1c\xea\x1a\xfb\xf0+F\x0cm\x1d'

An example usage of an encryption algorithm (AES, in this case) is:

>>> from Crypto.Cipher import AES
>>> obj = AES.new('This is a key456', AES.MODE_ECB)
>>> message = "The answer is no"
>>> ciphertext = obj.encrypt(message)
>>> ciphertext
'o\x1aq_{P+\xd0\x07\xce\x89\xd1=M\x989'
>>> obj2 = AES.new('This is a key456', AES.MODE_ECB)
>>> obj2.decrypt(ciphertext)
'The answer is no'

One possible application of the modules is writing secure
administration tools.  Another application is in writing daemons and
servers.  Clients and servers can encrypt the data being exchanged and
mutually authenticate themselves; daemons can encrypt private data for
added security.  Python also provides a pleasant framework for
prototyping and experimentation with cryptographic algorithms; thanks
to its arbitrary-length integers, public key algorithms are easily
implemented.

As of PyCrypto 2.1.0, PyCrypto provides an easy-to-use random number
generator:

>>> from Crypto import Random
>>> rndfile = Random.new()
>>> rndfile.read(16)
'\xf7.\x838{\x85\xa0\xd3>#}\xc6\xc2jJU'

A stronger version of Python's standard "random" module is also
provided:

>>> from Crypto.Random import random
>>> random.choice(['dogs', 'cats', 'bears'])
'bears'

Caveat: For the random number generator to work correctly, you must
call Random.atfork() in both the parent and child processes after
using os.fork()


