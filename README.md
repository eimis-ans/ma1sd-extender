# MA1SD-Extender

This project is a fork from <https://github.com/cameronwickes/ma1sd-extender> ❤️

<p align="center">
  <img alt="matrix logo" src="https://www.cameronwickes.co.uk/ma1sd-extender.png" width="250px"/>
</p>

![Supported Platforms](https://img.shields.io/badge/Platform-Linux-blueviolet?color=blue&style=for-the-badge)
![Language](https://img.shields.io/badge/Language-Python-blue?color=blueviolet&style=for-the-badge)
![License](https://img.shields.io/github/license/cameronwickes/ma1sd-extender?color=brightgreen&style=for-the-badge)

An API, built with **Docker** and **FastAPI**, that allows **Matrix** user directory searches to be recursively federated for corporate use.

---

MA1SD-Extender performs the following sequence of actions in order to recursively federate directory lookups:

- Checks the validity of API supplied credentials
- Checks the validity of a user specified authorization token against all federation domains
- Returns previously cached responses for faster lookups
- Searches within local directory for users
- Recursively searches other federation domains for users
- Returns pooled responses masquerading as the local MA1SD server

---

## ⚙️ Configuration Variables

The following environment variables are required by MA1SD-Extender:

- `MA1SD_EXTENDER_USERNAME`: The username for MA1SD-Extender access.
- `MA1SD_EXTENDER_PASSWORD`: The password for MA1SD-Extender access.
- `MA1SD_EXTENDER_MATRIX_DOMAIN`: The domain that Synapse and MA1SD are running on.
- `MA1SD_EXTENDER_FEDERATED_DOMAINS`: Any domains to be recursively federated.

MA1SD-Extender needs a valid account on the local Synapse homeserver and both MA1SD and Synapse need to be running the host that the extender runs on.

## 🐍 Running With Python

MA1SD-Extender can be run with Python, Poetry and FastAP with the following commands

```bash
export MA1SD_EXTENDER_USERNAME="X" \
MA1SD_EXTENDER_PASSWORD="X" \
MA1SD_EXTENDER_MATRIX_DOMAIN="X" \
MA1SD_EXTENDER_FEDERATED_DOMAINS="['X']"
poetry install
uvicorn --reload --host='0.0.0.0' --port=8060 ma1sd-extender.main:app
```

## 📦 Running With Docker/Podman

MA1SD-Extender can also be run with Docker/Podman with the following commands:

```bash
docker run --name ma1sd \
-e MA1SD_EXTENDER_USERNAME="X" \
-e MA1SD_EXTENDER_PASSWORD="X" \
-e MA1SD_EXTENDER_MATRIX_DOMAIN="X" \
-e MA1SD_EXTENDER_FEDERATED_DOMAINS="['X']" \
ghcr.io/eimis-ans/ma1sd-extender:latest
```

## 💻 NGINX Proxy Setup

Once MA1SD-Extender is running, an NGINX proxy can be configured to pass requests to and from the API. The following section should be placed within the NGINX configuration file.

```nginx
location /_matrix/client/r0/user_directory {
    proxy_pass http://0.0.0.0:8060/_matrix/client/r0/user_directory;
    proxy_set_header Host $host;
    proxy_set_header X-Forwarded-For $remote_addr;
}
```

## ⚖️ License

`MA1SD-Extender` is free and open-source software licensed under the [Apache 2.0 License](https://github.com/cameronwickes/ma1sd-extender/blob/main/LICENSE).
