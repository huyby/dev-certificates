# Create development certificates the easy way!

If you're a web developer and need to test your websites locally on HTTPs, you probably don't want to buy an SSL certificate for this.

Instead, you can **generate your own certificates!** 👍

However, working with the `openssl` CLI can be intimidating, and it's hard to remember the options. So I've put together these scripts and instructions to make your life easier!

## Download

Right click below, *Save Link As...*:

- create-ca.sh
- create-certificate.sh

## Generate your *Certificate Authority*

The Certificate Authority is what will make your browser trust the certificates that you'll generate for your development domains. Your browser is bundled with a list of Certificate Autorities that it trusts by default, but because you'll be signing your certificates yourself, we need to instruct it to trust your own certificates.

Just run:

```
$ ./create-ca.sh 
```

```
Generating RSA private key, 2048 bit long modulus (2 primes)
......................+++++
......................................+++++
e is 65537 (0x010001)
Success!

The following files have been written:
  - ca.crt is the public certificate that should be imported in your browser
  - ca.key is the private key that will be used by create-certificate.sh

Next steps:
  - Import ca.crt in your browser
  - run create-certificate.sh example.com
```

You only need to perform this step **once**.

## Import the CA in your browser

### In Chrome

- go to `chrome://settings/certificates`
- click the "Authorities" tab
- click "Import"
- select the `ca.crt` file
- check "Trust this certificate for identifying websites"
- click "OK"

### In Firefox

- go to `about:preferences#privacy`
- scroll down to "Certificates"
- click "View Certificates..."
- click "Import..."
- select the `ca.crt` file
- check "Trust this CA to identify websites"
- click "OK"

You only need to perform this step **once for each browser**.

## Generate your certificate

To generate a certificate for `example.dev` and its subdomains, run:

```
./create-certificate.sh example.dev
```

```
Generating RSA private key, 2048 bit long modulus (2 primes)
.................................+++++
..................................................................................................................................................................................................+++++
e is 65537 (0x010001)
Signature ok
subject=C = US, O = Local Development, CN = example.dev
Getting CA Private Key
Success!

You can now use example.dev.key and example.dev.crt in your web server.
Don't forget that you must have imported ca.crt in your browser to make it accept the certificate.
```

You can now install the `.key` and `.crt` files in your web server, such as Apache or Nginx.

You only need to perform this step **once per domain name**.

## That's it!

You should now be greeted with:

(image)

Enjoy!

## Credits
 
These scripts have been created from the steps highlighted in [this StackOverflow answer](https://stackoverflow.com/a/60516812/759866).