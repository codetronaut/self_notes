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
> Steps:

1. Generating GPG key pair.
```sh
$ gpg --full-generate-key

```
> ***Note:***
``` 
If you are not using atleast GPG 2.1.17 then the above command will not work so, paste this then:

$ gpg --default-new-key-algo rsa4096 --gen-key

```
and skip the steps for generating key above.


2. At the prompt, specify the kind of key you want, or press Enter to accept the default RSA and DSA and enter the desired key but keep in mind key must be at least 4096 bits.

3. Enter the length of time the key should be valid. Press Enter to specify the default selection, indicating that the key doesn't expire.

4. Enter your user ID information.

5. Type a secure passphrase.

6. Use the `gpg --list-secret-keys --keyid-format LONG` command to list GPG keys for which you have both a public and private key. A private key is required for signing commits or tags.
```sh
$ gpg --list-secret-keys --keyid-format LONG
```
7. From the list of GPG keys, copy the GPG key ID you'd like to use. In this example, the GPG key ID is 3AA5C34371567BD2:

```sh
$ gpg --list-secret-keys --keyid-format LONG
/Users/hubot/.gnupg/secring.gpg
------------------------------------
sec   4096R/3AA5C34371567BD2 2016-03-10 [expires: 2017-03-10]
uid                          Hubot 
ssb   4096R/42B317FD4BA89E7A 2016-03-10
```

8. Paste the text below, substituting in the GPG key ID you'd like to use. In this example, the GPG key ID is 3AA5C34371567BD2:
```sh
$ gpg --armor --export <your-generated-key>
# Prints the GPG key ID, in ASCII armor format
e.g
$ gpg --armor --export 3AA5C34371567BD2
```
9. Copy your GPG key, beginning with `-----BEGIN PGP PUBLIC KEY BLOCK-----` and ending with `-----END PGP PUBLIC KEY BLOCK-----`

10. And follw [Github's instructions](https://help.github.com/en/github/authenticating-to-github/adding-a-new-gpg-key-to-your-github-account) on how to add a GPG  key to Github.

11. Once done with step-10 let's jumo to the other part i.e

# sign commits
For signing commit you can either use 
```sh
$ git commit -S -m "Commit signed check"
```
whenever you put `-S` it will sign that particular commit.

if you want to sign all future commits then run this command
```sh
$ git config --global commit.gpgsign true
```
and yeah that's it now commit to your repo  and see the (cool)verified tag on all your commits.

> ***Note:***
Make sure that you have entered right details in your .gitconfig file.

for doing that you have to do some steps below:
```sh
$ git config --global user.name "Your Name"
$ git config --global user.email "your@mail.com"
```
That's it now your commits are verified and you are good to go for contributing to any opensource organisation as a trusworthy.

want to know why verification of commits are important [see here](https://github.blog/2016-04-05-gpg-signature-verification/).

until then 
peace!



Author: Anmol
