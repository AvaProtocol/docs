# nucypher-note
1. clef, docker nucypher connection still exist error:
https://discord.com/channels/411401661714792449/497481850382450710/750396446418927686

## Setup
1. pip3 install virtualenv
2. virtualenv /Users/crazyguy/nucypher-venv
3. source /Users/crazyguy/nucypher-venv/bin/activate
4. pip3 install -U nucypher
5. nucypher --help

check nucypher is installed success, eg:
```
(nucypher-venv)  ~  nucypher --help
Usage: nucypher [OPTIONS] COMMAND [ARGS]...

  Top level command for all things nucypher.

Options:
  --version  Echo the CLI version
  --help     Show this message and exit.

Commands:
  alice     "Alice the Policy Authority" management commands.
  bob       "Bob the Data Recipient" management commands.
  enrico    "Enrico the Encryptor" management commands.
  felix     "Felix the Faucet" management commands.
  multisig  Perform operations on NuCypher contracts via a MultiSig
  stake     Manage stakes and other staker-related operations.
  status    Echo a snapshot of live NuCypher Network metadata.
  ursula    "Ursula the Untrusted" PRE Re-encryption node management...
  worklock  Participate in NuCypher's WorkLock to obtain a NU stake
```

## Clef

because we use remote provider like infura, so we need to use clef to sign transaction; in macos, we can ```brew install ethereum```;
check clef is installed success, run ```clef```, eg:
```
 ~  clef

WARNING!

Clef is an account management tool. It may, like any software, contain bugs.

Please take care to
- backup your keystore files,
- verify that the keystore(s) can be opened with your password.

Clef is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY;
without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR
PURPOSE. See the GNU General Public License for more details.

Enter 'ok' to proceed:
```

we maybe need to import account, we can run ```geth account import ./privatekey``` that ./privatekey contains privatekey;

## Worklock

1. run ```nucypher worklock status --network ibex --provider https://rinkeby.infura.io/v3/90387f685e4d4dfb9b8a9e345985766d``` to check provider is correct to use, eg: 
```
(nucypher-venv)  ~  nucypher worklock status --network ibex --provider https://rinkeby.infura.io/v3/90387f685e4d4dfb9b8a9e345985766d
Reading Latest Chaindata...

WorkLock (0xb9EB79FdE9D1cd77d876ce52364a701944094166)

Time
══════════════════════════════════════════════════════

Escrow Period (Closed)
------------------------------------------------------
Allocations Available . Yes
Start Date ............ 2020-06-13 00:00:00+00:00
End Date .............. 2020-06-19 23:59:59+00:00
Duration .............. 6 days, 23:59:59
Time Remaining ........ Closed

Cancellation Period (Closed)
------------------------------------------------------
End Date .............. 2020-06-20 23:59:59+00:00
Duration .............. 7 days, 23:59:59
Time Remaining ........ Closed


Economics
══════════════════════════════════════════════════════

Participation
------------------------------------------------------
Lot Size .............. 250000000 NU
Min. Allowed Escrow ... 1 ETH
Participants .......... 32
ETH Supply ............ 276.791 ETH
ETH Pool .............. 269167567500110133332 wei

Base (minimum escrow)
------------------------------------------------------
Base Deposit Rate ..... 15000 NU per base ETH

Bonus (surplus over minimum escrow)
------------------------------------------------------
Bonus ETH Supply ...... 244.791 ETH
Bonus Lot Size ........ 249520000 NU
Bonus Deposit Rate .... 1019318 NU per bonus ETH

Refunds
------------------------------------------------------
Refund Rate Multiple .. 4.00
Bonus Refund Rate ..... 254829.5 units of work to unlock 1 bonus ETH
Base Refund Rate ...... 3750.0 units of work to unlock 1 base ETH

    * NOTE: bonus ETH is refunded before base ETH
```

## Staker

before do stake, make sure clef is running:
```
 ~  clef --chainid 4 --advanced --rules rules.js

WARNING!

Clef is an account management tool. It may, like any software, contain bugs.

Please take care to
- backup your keystore files,
- verify that the keystore(s) can be opened with your password.

Clef is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY;
without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR
PURPOSE. See the GNU General Public License for more details.

Enter 'ok' to proceed:
> ok

INFO [09-02|20:58:53.710] Using CLI as UI-channel
INFO [09-02|20:58:53.875] Loaded 4byte database                    embeds=146841 locals=0 local=./4byte-custom.json
## Master Password

Please enter the password to decrypt the master seed
>
-----------------------
WARN [09-02|20:58:59.224] Could not load rules, disabling          file=rules.js err="open rules.js: no such file or directory"
INFO [09-02|20:58:59.224] Starting signer                          chainid=4 keystore=/Users/crazyguy/Library/Ethereum/keystore light-kdf=false advanced=true
DEBUG[09-02|20:58:59.226] FS scan times                            list=1.648499ms set="9.023µs" diff="4.43µs"
DEBUG[09-02|20:58:59.242] Ledger support enabled
DEBUG[09-02|20:58:59.245] Trezor support enabled via HID
DEBUG[09-02|20:58:59.247] Trezor support enabled via WebUSB
INFO [09-02|20:58:59.248] Clef is in advanced mode: will warn instead of reject
INFO [09-02|20:58:59.248] Audit logs configured                    file=audit.log
DEBUG[09-02|20:58:59.248] IPC registered                           namespace=account
INFO [09-02|20:58:59.249] IPC endpoint opened                      url=/Users/crazyguy/Library/Signer/clef.ipc
------- Signer info -------
* intapi_version : 7.0.1
* extapi_version : 6.0.0
* extapi_http : n/a
* extapi_ipc : /Users/crazyguy/Library/Signer/clef.ipc
```

1. init stakeholder
```
nucypher stake init-stakeholder --signer clef:///Users/crazyguy/Library/Signer/clef.ipc --provider https://rinkeby.infura.io/v3/90387f685e4d4dfb9b8a9e345985766d --network ibex
Wrote new stakeholder configuration to /Users/crazyguy/Library/Application Support/nucypher/stakeholder.json
```

2. create stake
```
nucypher stake create --hw-wallet
    Account
--  ------------------------------------------
 0  0x173F7E406b6Acf6df7c15e7ae9bD23e663f0Aa73
 1  0x83914c693b379175b19E234c1d2f8e09d6368656
Select index of staking account [0]: 1
Selected 1: 0x83914c693b379175b19E234c1d2f8e09d6368656
Transfer tokens from the staker address? Otherwise, unlocked tokens in the escrow will be used [True]: True
Enter stake value in NU (15000 NU - 45000 NU) [45000]: 45000
Enter stake duration (30 - 47028) [365]: 365

══════════════════════════════ STAGED STAKE ══════════════════════════════

Staking address: 0x83914c693b379175b19E234c1d2f8e09d6368656
~ Chain      -> ID # 4 | Rinkeby
~ Value      -> 45000 NU (45000000000000000000000 NuNits)
~ Duration   -> 365 Days (365 Periods)
~ Enactment  -> Sep 03 08:00 CST (period #18508)
~ Expiration -> Sep 03 08:00 CST (period #18873)

═════════════════════════════════════════════════════════════════════════

* Ursula Node Operator Notice *
-------------------------------

By agreeing to stake 45000 NU (45000000000000000000000 NuNits):

- Staked tokens will be locked for the stake duration.

- You are obligated to maintain a networked and available Ursula-Worker node
  bonded to the staker address 0x83914c693b379175b19E234c1d2f8e09d6368656 for the duration
  of the stake(s) (365 periods).

- Agree to allow NuCypher network users to carry out uninterrupted re-encryption
  work orders at-will without interference.

Failure to keep your node online or fulfill re-encryption work orders will result
in loss of staked NU as described in the NuCypher slashing protocol:
https://docs.nucypher.com/en/latest/architecture/slashing.html.

Keeping your Ursula node online during the staking period and successfully
producing correct re-encryption work orders will result in rewards
paid out in ethers retro-actively and on-demand.

Accept ursula node operator obligation? [y/N]: y
Publish staged stake to the blockchain? [y/N]: y
Confirm transaction APPROVEANDCALL on hardware wallet... (0.13969263 ETH @ 489 gwei)
Broadcasting APPROVEANDCALL Transaction (0.13969263 ETH @ 489 gwei)...

Stake initialization transaction was successful.

Transaction details:
OK | deposit stake | 0xc83ec0d8d442fba859367dc7fd058201a75ba769ed87832f2f14ad239fd6046e (244335 gas)
Block #7124644 | 0xe15a8024cee5e644c56c6dcab5d1228db1a0a4230c7297bcd2f48912a94aff69
 See https://rinkeby.etherscan.io/tx/0xc83ec0d8d442fba859367dc7fd058201a75ba769ed87832f2f14ad239fd6046e


StakingEscrow address: 0x6A6F917a3FF3d33d26BB4743140F205486cD6B4B

View your stakes by running 'nucypher stake list'
or set your Ursula worker node address by running 'nucypher stake bond-worker'.

See https://docs.nucypher.com/en/latest/guides/network_node/staking_guide.html
```

after stake, can run 
```
nucypher stake list

Network Ibex ═════════════════════════════════════════
Staker 0x83914c693b379175b19E234c1d2f8e09d6368656 ════
Worker 0xE2bb6Ae7741f9C975E676C1Bd26f7503Cdb36377 ════
--------------  ---------------------
Status          Missing 1 commitments
Restaking       Yes (Unlocked)
Winding Down    No
Unclaimed Fees  0 ETH
Min fee rate    0 ETH
--------------  ---------------------
╒═══════╤══════════╤═════════════╤═════════════╤═══════════════╤═══════════╕
│   Idx │ Value    │   Remaining │ Enactment   │ Termination   │ Status    │
╞═══════╪══════════╪═════════════╪═════════════╪═══════════════╪═══════════╡
│     0 │ 45000 NU │         366 │ Sep 03 2020 │ Sep 05 2021   │ DIVISIBLE │
╘═══════╧══════════╧═════════════╧═════════════╧═══════════════╧═══════════╛
```
to check it

3. bond a worker to a staker
```
nucypher stake bond-worker --hw-wallet
```

## Worker
1. ursula init
```
nucypher ursula init --provider https://rinkeby.infura.io/v3/90387f685e4d4dfb9b8a9e345985766d --network ibex --signer clef:///Users/crazyguy/Library/Signer/clef.ipc
```

2. worker run
```
(nucypher-venv)  ~  nucypher ursula run --interactive
Enter password to unlock account 0xE2bb6Ae7741f9C975E676C1Bd26f7503Cdb36377:
Enter NuCypher keyring password:
Decrypting ursula keyring...
Connecting to preferred teacher nodes...
Waiting for bonding and funding...
Worker is bonded to (0x83914c693b379175b19E234c1d2f8e09d6368656)!
Worker is funded with 6.9311332626 ETH!
Starting services...
✓ Database pruning
✓ Node Discovery (ibex)
✓ Availability Checks
✓ Work Tracking

Type 'help' or '?' for help
Starting Ursula on 125.118.113.214:9151
Working ~ Keep Ursula Online!
Ursula(0x83914c6) >>> status

⇀URSULA ⛨ ☿↽
(Ursula)⇀DarkOliveGreen Shield LightCoral Mercury↽ (0x83914c693b379175b19E234c1d2f8e09d6368656)
Uptime .............. 0:00:17
Start Time .......... 16 seconds ago
Fleet State.......... 492ed01 ⇀Cyan Airplane↽ ✈
Learning Status ..... Learning at 5s Intervals
Learning Round ...... Round #5
Operating Mode ...... Decentralized
Rest Interface ...... 125.118.113.214:9151
Node Storage Type ... Local
Known Nodes ......... 1155
Work Orders ......... 0
Current Teacher ..... (NodeSprout)⇀Tan Spade DarkGray Flag↽ (0x031C9c11CD95Dd82e9395B9A268d9A90656acE8e)
Current Period ...... 18507
Worker Address ...... 0xE2bb6Ae7741f9C975E676C1Bd26f7503Cdb36377
Availability Score .. 10 (0 responders)

Ursula(0x83914c6) >>>
```
