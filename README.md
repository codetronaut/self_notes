# Set up GPG & Git to sign commits on GitHub

Step-by-step guide on how to create a GPG key and adding it to the a local GPG setup and use it with Git and any Git platform GitHub, Gitlab or even if you want to send patches through mailing-lists.


Although this guide was written for Linux(specifically debian and debian based like ubuntu), most of the commands should work in other operating systems as well.


So let's start:

# Generating a new GPG key

Generally Github supports several GPG key algorithms like RSA, ElGamal, DSA e.t.c and these should also work with other git platforms as well.

## Requirements
```sh
$ sudo apt update
$ sudo apt install gnupg 
```
Generating GPG key pair.
```sh
$ gpg --full-generate-key
```
-> At the prompt, specify the kind of key you want, or press Enter to accept the default RSA and DSA and enter the desired key but keep in mind key must be at least 4096 bits.

-> Enter the length of time the key should be valid. Press Enter to specify the default selection, indicating that the key doesn't expire.

-> Enter your user ID information.

-> Type a secure passphrase.

-> Use the `gpg --list-secret-keys --keyid-format LONG` command to list GPG keys for which you have both a public and private key. A private key is required for signing commits or tags.
```sh
$ gpg --list-secret-keys --keyid-format LONG
```
-> From the list of GPG keys, copy the GPG key ID you'd like to use. In this example, the GPG key ID is 3AA5C34371567BD2:

```sh
$ gpg --list-secret-keys --keyid-format LONG
/Users/hubot/.gnupg/secring.gpg
------------------------------------
sec   4096R/3AA5C34371567BD2 2016-03-10 [expires: 2017-03-10]
uid                          Hubot 
ssb   4096R/42B317FD4BA89E7A 2016-03-10
```

-> Paste the text below, substituting in the GPG key ID you'd like to use. In this example, the GPG key ID is 3AA5C34371567BD2:
```sh
$ gpg --armor --export 3AA5C34371567BD2
# Prints the GPG key ID, in ASCII armor format
```
-> Copy your GPG key, beginning with `-----BEGIN PGP PUBLIC KEY BLOCK-----` and ending with `-----END PGP PUBLIC KEY BLOCK-----`


> ***Note:***
If you are not using atleast GPG 2.1.17 then the above command will not work paste this then:
```sh
$ gpg --default-new-key-algo rsa4096 --gen-key
```

(work in progress)

