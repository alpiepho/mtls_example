# MTLS NOTES

This is my hands-on followup to a video I stumbled across that described how the certificate chain mechanism works and how it can be used for Mutual TLS connections, where both sides validate the other.

- from ZephyrOS video
    - https://www.youtube.com/watch?v=8-PU9_ONSrY
- from Linaro video
    - https://resources.linaro.org/en
    - https://static.sched.com/hosted_files/zephyr2022/56/Zephyr%20F2F%2022%20X.509%20Client%20Auth.pdf
    - "Confidential AI: Secure Data Processing Pipeline"

- code example
    - https://gist.github.com/microbuilder/cf928ea5b751e6ea467cc0cd51d2532f#file-certgen-sh=



<hr>
# ORIGINAL _mtls.md

# Setting up the mutual TLS server project

Initialise a new project:

```bash
$ go mod init github.com/<yourusername>/gomtls
```

Insert the go source code in this gist in `main.go`:

```bash
$ curl https://gist.githubusercontent.com/microbuilder/cf928ea5b751e6ea467cc0cd51d2532f/raw/main.go \
  --output main.go
```

Create keys/certs in the `certs` folder with `certgen.sh`

> **IMPORTANT**: Update this script with the correct `HOSTNAME` value before running it!

```bash
$ mkdir certs && cd certs
$ curl https://gist.githubusercontent.com/microbuilder/cf928ea5b751e6ea467cc0cd51d2532f/raw/certgen.sh \
  --output certgen.sh 
$ chmod +x certgen.sh
$ ./certgen.sh
$ cd ..
```

Start the server (check the hostname it displays!):

> NOTE: Run `export CAHOSTNAME='localhost'` (etc.) before starting
> the application to change the default hostname!

```bash
$ go run main.go
```

In a new terminal, connect with the device (client) certificate:

> Set the hostname in `-connect` to an appropriate value.
> The server should display the hostname used at startup,
> which needs to match the hostname in the server certificate.

```bash
$ openssl s_client \
  -cert certs/DEVICE.crt \
  -key certs/DEVICE.key \
  -CAfile certs/CA.crt \
  -connect localhost:8443
```
