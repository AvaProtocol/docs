clef Passphrase: bkg12345678

## WorkLock

1. docker run -it -v ~/.local/share/nucypher:/root/.local/share/nucypher -v ~/.ethereum/:/root/.ethereum -p 9151:9151 nucypher/nucypher:latest nucypher worklock status --network ibex --provider https://rinkeby.infura.io/v3/90387f685e4d4dfb9b8a9e345985766d

network: ibex, mainnet

## Staker

1. Using Clef
```
clef --networkid 4 --advanced --rules rules.js

WARNING!

Clef is alpha software, and not yet publically released. This software has _not_ been audited, and there
are no guarantees about the workings of this software. It may contain severe flaws. You should not use this software
unless you agree to take full responsibility for doing so, and know what you are doing.

TLDR; THIS IS NOT PRODUCTION-READY SOFTWARE!


Enter 'ok' to proceed:
>ok
INFO [09-01|20:59:45.777] Using CLI as UI-channel
INFO [09-01|20:59:45.789] Loaded 4byte db                          signatures=6781 file=./4byte.json local=./4byte-custom.json
Master Password
Please enter the password to decrypt the master seed
-----------------------
INFO [09-01|20:59:51.492] Could not validate ruleset hash, rules not enabled got=2082efea1ac5d809d04da533d8754685f43bac1e68532b1c7d44fc1365680d2b expected=8d089001fbb55eb8d9661b04be36ce3285ffa940e5cdf248d0071620cf02ebcd
DEBUG[09-01|20:59:51.494] FS scan times                            list=908.921µs set=358.838µs diff=15.541µs
INFO [09-01|20:59:51.495] Clef is in advanced mode: will warn instead of reject
DEBUG[09-01|20:59:51.506] Ledger support enabled
DEBUG[09-01|20:59:51.508] Trezor support enabled
INFO [09-01|20:59:51.509] Audit logs configured                    file=audit.log
DEBUG[09-01|20:59:51.512] IPC registered                           namespace=account
INFO [09-01|20:59:51.514] IPC endpoint opened                      url=/Users/crazyguy/Library/Signer/clef.ipc
------- Signer info -------
* extapi_version : 4.0.0
* intapi_version : 3.0.0
* extapi_http : n/a
* extapi_ipc : /Users/crazyguy/Library/Signer/clef.ipc
```

2. Initialize a new stakeholder
```
docker run -it -v ~/.local/share/nucypher:/root/.local/share/nucypher -v ~/.ethereum/:/root/.ethereum -p 9151:9151 nucypher/nucypher:latest nucypher stake init-stakeholder --signer ipc:///Users/crazyguy/Library/Signer/clef.ipc --provider https://rinkeby.infura.io/v3/90387f685e4d4dfb9b8a9e345985766d --network ibex
Wrote new stakeholder configuration to /root/.local/share/nucypher/stakeholder.json
```

3. Initialize a new stake (IPC Connection Failed)
```
docker run -it -v ~/.local/share/nucypher:/root/.local/share/nucypher -v ~/.ethereum/:/root/.ethereum -p 9151:9151 nucypher/nucypher:latest nucypher stake create --hw-wallet

Traceback (most recent call last):
  File "/usr/local/lib/python3.7/site-packages/nucypher/blockchain/eth/interfaces.py", line 996, in get_interface
    cached_interface = cls._interfaces[provider_uri]
KeyError: 'ipc://Users/crazyguy/Library/Signer/clef.ipc'

...

nucypher.blockchain.eth.interfaces.ConnectionFailed: Connection Failed - ipc:///Users/crazyguy/Library/Signer/clef.ipc - is IPC enabled?
```

## Worker
当前走到第5步，卡住了；原因如上；

1. Ensure that a Stake is available (see Staker Configuration Guide)

2. Run an ethereum node on the Worker’s machine eg. geth, parity, etc. (see Running an Ethereum node for Ursula)

3. Install nucypher on Worker node (see Installation Guide)

4. Create and fund worker’s ethereum address (see Fund Worker Account with ETH)

5. Bond the Worker to a Staker (see Bonding a Worker)

6. Configure and run a Worker node (see Configure and Run Ursula)

7. Ensure TCP port 9151 is externally accessible (see Ursula / Worker Requirements)

8. Keep Worker node online!