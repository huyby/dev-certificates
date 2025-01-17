# Create development certificates the easy way!

<img src="https://raw.githubusercontent.com/BenMorel/dev-certificates/main/images/logo.svg" alt="" align="left" height="150">

If you're a web developer and need to test your websites locally on HTTPs, you probably don't want to buy an SSL certificate for this.

Instead, you want to **generate your own certificates**! 💪

However, working with the `openssl` CLI can be intimidating. Options are hard to remember. So I've put together these scripts and instructions to make my life, and yours, easier!

## Download

Right click below, *Save Link As...*:

- [create-ca.sh](https://raw.githubusercontent.com/BenMorel/dev-certificates/main/create-ca.sh)
- [create-certificate.sh](https://raw.githubusercontent.com/BenMorel/dev-certificates/main/create-certificate.sh)

## Generate your own Certificate Authority

The *Certificate Authority* is what will make your browser trust the certificates that you'll generate for your development domains. Your browser is bundled with a list of Certificate Autorities that it trusts by default, but because you'll be signing your certificates yourself, you need to instruct it to trust your own certificates.

Just run:

```
create-ca.sh 
```

![create-ca.sh](https://raw.githubusercontent.com/BenMorel/dev-certificates/main/images/create-ca.png)

You only need to perform this step **once**.

## Generate a certificate for your domain

To generate a certificate for `example.dev` and its subdomains, run:

```
create-certificate.sh example.dev
```

To generate a certificate for `example.dev` along with one or more [alternative domains](https://en.wikipedia.org/wiki/Subject_Alternative_Name), run:

```
create-certificate.sh example.dev alternative-domain.dev alternative-domain.test
```

![create-certificate.sh](https://raw.githubusercontent.com/BenMorel/dev-certificates/main/images/create-certificate.png)

You can now install the `.key` and `.crt` files in your web server, such as Apache or Nginx.

Repeat this step if you need certificates for other domain names.

## Import the CA in your browser

You only need to perform this step **once** for each browser, whatever the number of certificates you'll generate.

### <img src="https://raw.githubusercontent.com/BenMorel/dev-certificates/main/images/windows.svg" alt="" align="center" height="24"> Edge & Chrome on Windows 10

- open `certmgr.msc`
- expand "Trusted Root Certification Authorities"
- right-click on "Certificates", then "All Tasks" > "Import..."
- click "Next"
- choose the `ca.crt` file
- click "Next" then "Finish"

### <img src="https://raw.githubusercontent.com/BenMorel/dev-certificates/main/images/chrome.svg" alt="" align="center" height="24"> Chrome on Linux

- go to `chrome://settings/certificates`
- click the "Authorities" tab
- click "Import"
- select the `ca.crt` file
- check "Trust this certificate for identifying websites"
- click "OK"

### <img src="https://raw.githubusercontent.com/BenMorel/dev-certificates/main/images/firefox.svg" alt="" align="center" height="24"> Firefox on Windows & Linux

- go to `about:preferences#privacy`
- scroll down to "Certificates"
- click "View Certificates..."
- click "Import..."
- select the `ca.crt` file
- check "Trust this CA to identify websites"
- click "OK"

### <img src="https://raw.githubusercontent.com/BenMorel/dev-certificates/main/images/safari.svg" alt="" align="center" height="24"> Safari, Chrome & Edge on macOS

Safari, Chrome, Edge and Opera all use the built-in MacOS [Keychain Access](https://support.apple.com/guide/keychain-access/what-is-keychain-access-kyca1083/mac) application to manage trusted root certificates. Run the following from your terminal/shell to add `ca.crt` as a trusted root certificate:

    sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain ca.crt

### <img src="https://raw.githubusercontent.com/BenMorel/dev-certificates/main/images/apple.svg" alt="" align="center" height="24"> Safari, Chrome & Firefox on iOS

- Download your certificate:
  - host your `ca.crt` certificate file somewhere on the web (I use [gist.github.com](https://gist.github.com/))
  - open the URL to the certificate in your mobile browser (I [generate a QR code](https://qr-code-generator.com/) to the raw gist URL)
  - You should be asked "This website is trying to download a configuration profile. Do you want to allow this?", tap "Allow"
- Install the certificate:
  - Open the Settings app
  - Underneath your name, tap "Profile Downloaded"
  - In the top right corner, tap "Install"
  - Enter your passcode
  - In the warning page that opens, tap "Install" again
- Enable the certificate for websites:
  - Go back to Settings > General > About > Certificate Trust Settings
  - In the "Enable Full Trust for Root Certificates" section, enable the certificate you just installed
  - In the warning that opens, click "Continue"

### <img src="https://raw.githubusercontent.com/BenMorel/dev-certificates/main/images/android.svg" alt="" align="center" height="24"> <img src="https://raw.githubusercontent.com/BenMorel/dev-certificates/main/images/chrome.svg" alt="" align="center" height="24"> Chrome on Android

- Download your certificate:
  - host your `ca.crt` certificate file somewhere on the web (I use [gist.github.com](https://gist.github.com/))
  - open the URL to the certificate in your mobile browser (I [generate a QR code](https://qr-code-generator.com/) to the raw gist URL)
  - Click the Download button in the top bar
- Install the certificate:
  - Go to Files > Downloads
  - Click the `ca.crt` file
  - Enter your PIN when prompted
  - Give a name to the certificate
  - Click OK

### <img src="https://raw.githubusercontent.com/BenMorel/dev-certificates/main/images/android.svg" alt="" align="center" height="24"> <img src="https://raw.githubusercontent.com/BenMorel/dev-certificates/main/images/firefox.svg" alt="" align="center" height="24"> Firefox on Android

Firefox does not use Android's certificate store, but does not seem to provide a way to import a certificate. You may wish to add your voice to the [bug tracker](https://bugzilla.mozilla.org/show_bug.cgi?id=868370) if you wish to see this implemented.

## That's it!

You should now be greeted with:

![Chrome Secure](https://raw.githubusercontent.com/BenMorel/dev-certificates/main/images/secure.png)

If you need to create certificates for other domains, just run `create-certificate.sh` again.
**No need to create or import the CA again!**

Enjoy! 👋

## Credits

These scripts have been created from the steps highlighted in [this StackOverflow answer](https://stackoverflow.com/a/60516812/759866) by [@entrity](https://github.com/entrity).

Icons by [Pixel perfect](https://www.flaticon.com/authors/pixel-perfect).
