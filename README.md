## Generic Faucet for Substrate based chains

## Development

Setup dependencies and git hooks

```bash
yarn install
yarn simple-git-hooks
```

To launch a hot-reloading dev environment

```bash
yarn dev:backend
yarn dev:bot
```

## Server and Bot environment variables

Definition with explanation is in `./env.bot.config.yml` and `./env.server.config.yml`

Copy example file to real env and change its values:
```bash
$ cp example.env .env
```

## Run the faucet locally for troubleshooting

Use the following commands to run a local instance of the faucet built directly from sources:

  cd docker/
  export SMF_BACKEND_FAUCET_ACCOUNT_MNEMONIC=***
  export SMF_BOT_MATRIX_BOT_USER_ID=***
  export SMF_BOT_MATRIX_ACCESS_TOKEN=***
  docker-compose -f docker-compose.<network>.yml up

Note: You will need a valid funded account mnemonic and matrix user ID / access token.

## Helm deployment / Adding a new faucet

0. Create an account for your SMF_BOT_MATRIX_BOT_USER_ID at https://matrix.org/, login and retrieve SMF_BOT_MATRIX_ACCESS_TOKEN in `Settings -> Help and about -> click to reveal`
1. Create a *kubernetes/chainName-values.yaml* file and define all non default variables. Secret variables (SMF_BOT_MATRIX_ACCESS_TOKEN & SMF_BACKEND_FAUCET_ACCOUNT_MNEMONIC) you need to supply externally
via CI / command line / ...
2. Create a new CI-Job / Environment in *.gitlab-ci.yml* file and add Secrets (in clear / non-base64 encoded format) to `gitlab -> CI/CD Settings -> Secret Variables`).
3. Run CI/CD or use `helm` to deploy.

### Helm chart

An official [substrate-faucet helm chart](https://github.com/paritytech/helm-charts/tree/main/charts/substrate-faucet) is available for deploying the faucet.

### Misc:

* Bump API: `yarn upgrade @polkadot/util@latest @polkadot/wasm-crypto@latest @polkadot/keyring@latest @polkadot/x-randomvalues@latest @polkadot/api@latest @polkadot/keyring@latest @polkadot/util-crypto@latest`
* Server can be queried for Prometheus metrics via http://$SMF_BOT_BACKEND_URL/metrics
* Readiness check URL  via http://$SMF_BOT_BACKEND_URL/ready
* Health check URL  via http://$SMF_BOT_BACKEND_URL/health
