# TSH TOTP Wrapper

This project is a very simple wrapper for the [teleport tsh tool](https://github.com/gravitational/teleport) to programmatically supply the required time based otp codes.

## Getting Started

Just clone the script, set up your secret file, set up the environment variables if needed and good to go.

### Prerequisites/Installing

tsh tool - Provided by your [teleport](https://github.com/gravitational/teleport) installation

[expect](http://expect.sourceforge.net/) - Scripting to work with the tool interactively

```
$ apt-get install expect
$ pacman -S expect
```

[openssl](https://www.openssl.org/) - Store the secret securely

```
$ apt-get install openssl
$ pacman -S openssl
```

[oathtool](http://www.nongnu.org/oath-toolkit/) - Get the otp codes from the secret
```
$ apt-get install oathtool
$ pacman -S oath-toolkit
```

### Secret file

The otp codes are based on a secret that is given to you when you get your account. Usually this secret is a QR code that you can read with your smartphone.  
This QR has your secret and some other information, such as how often the otp code changes and how many digits it has.  
You need to read the QR and store the secret in an encrypted file.  

To read it, you can use zbar:
```
$ zbarimg -q --raw qr.png | openssl aes-256-cbc -e -md sha256 -out secret.aes
```
don't forget to erase any traces of the qr from your system.


This script will decrypt the secret file using the password provided for tsh and try to extract the parameters `secret` `digits` and `period` as if the secret file stored an uri.
If `secret` or `period` fails, it will use the defaults (6 and 30), if it cannot find `secret`, it will use the whole file as the `secret`.

Uri example:
```
otpauth://totp/ACME:roadrunner@auth-acme?algorithm=SHA1&digits=6&issuer=ACME&period=30&secret=MYSECRET
```


## Recommended configuration

You can set up the `TSHWRAPPER_SECRET_PATH` env var to point to your secret file or place it in `~/.tsh_otp_secret.aes`  
Set `TSHWRAPPER_SILENT` to 1 to have a very silent script  
Set `TSHWRAPPER_TSH_PATH` to your runnable tsh tool  
Have this script in your path with the name "tsh" (soft link, copy in ~/bin...)  
Now you have a tsh tool that automatically injects the otp codes!  

## Authors

* **Jose M Perez Ramos** - [Kuroneer](https://github.com/Kuroneer)

## License

This project is 114 lines long and thus it's released to the public domain.

