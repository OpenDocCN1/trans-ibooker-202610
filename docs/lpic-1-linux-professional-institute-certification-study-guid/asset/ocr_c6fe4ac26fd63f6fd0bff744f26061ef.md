# # Generating Keys

To begin using GPG, you'll need to generate a public/private key pair. GPG uses the typical asynchronous keys, where the public key is available publicly but the private key is kept private. (GPG calls the private key a _secret key_.) Start the process to generate your key pair by using the following command:

```
gpg --gen-key
```

GPG will ask a series of questions, including your full name and email address, as well as a passphrase. You'll need to remember the email address and the passphrase because the address identifies your public key, whereas the passphrase allows you to use your private key.

![](bbox=[0.13, 0.34, 0.22, 0.395])

The key generation command needs a lot of random data in order to produce strong keys. During this process, you are asked to type on the keyboard, move the mouse, press function keys, and so on. It takes a long time to complete. An easier way is to employ the random data tool (rngd). Install it from the rng-utils or rng-tools package. Before issuing the gpg --gen-key command, use super user privileges and type rngd -r /dev/urandom. After you receive the shell prompt back, issue the gpg --gen-key command. Though it will pause a moment or two at the part where the program is asking you to type on the keyboard, it will complete fairly quickly. And it is quicker than you trying to manually add random data.

After the keys are generated, they are stored in a file, which is called your keyring in the ~/.gnupg/ directory.

For someone to encrypt a file for you to decrypt, you'll need to make a copy of your public key for them. This is called exporting your key, and it will put the copy of the key into a file. The command to enact this is

```
gpg --export EMAIL-ADDRESS > FILENAME.pub
```

The EMAIL-ADDRESS identifies your public key on your keyring, and it was entered when you generate the key. The FILENAME can be anything. An example of this is shown in Listing 10.43.

# **Listing 10.43: Exporting a public key using the gpg --export command**

```
$ gpg --export cb1234@ivytech.edu > CB_gpg.pub
$
```

After you export (copy) your public key to a file, you can give it to the people who want to encrypt files to send to you. There is no reason to keep the public key secret, so you can email it or, if you have a public web page, place it there.