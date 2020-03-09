tBTC-MCD integration
====================

Adding TBTC as a collateral type to MCD.  #MuchCrosschainDAI

# Install

```
# Clone submodules.
git submodule update --init --recursive
```

Follow install instructions for each of the `dai.js`, `dss-add-ilk-spell`, `mcd-cdp-portal` and `testchain` projects.

## Running the MCD testchain

```
cd testchain
scripts/launch -s default --fast

# Update addresses 
cd ../dss-add-ilk-spell
cd deployments/download-testchain.sh
cd ..
```

## Deploying the spell.
 1. Copy the TBTC address from the `TBTCToken.json` or elsewhere.

 2. Run `./1-deploy-spell.sh TBTC_ADDRESS`

3. Ctrl+F for MCD_JOIN_TBTC_A 0x509627e7904bcfa76f8e96d1e364a8e26263bafa. 

   eg. `MCD_JOIN_TBTC_A 0x509627e7904bcfa76f8e96d1e364a8e26263bafa`.

   Copy for later.

4. Ctrl+F for Spell
   
   eg. `SPELL 0xa65f285c8b186068c6d9ada0f76ffdf53286a0dc`

   Copy for later.

5. Run `./2-setup-voting.sh` to add MKR approval to the governance contracts.

6. Run `./3-cast-spell.sh SPELL_ADDRESS` to cast the spell.


**NOTE:** Spell casting will fail if collateral with same name (eg. TBTC-A) is already voted in.

## Integrating in the MCD Oasis dApp.

### dai.js

Edit in `dai-plugin-mcd/contracts/addresses/testnet.json`:

1. `TBTC` - put the TBTC token address
2. `MCD_JOIN_TBTC_A` - search 'MCD_JOIN_TBTC_A' from your logs of running `1-deploy-spell.sh`, copy the address here

To rebuild:

```sh
cd dai.js
yarn build
```

### mcd-cdp-portal

Now we rebuild the Oasis dApp for local usage.

```sh
cd mcd-cdp-portal/
yarn add file:../dai.js/packages/dai file:../dai.js/packages/dai-plugin-mcd
yarn start
```

Visit http://localhost:3001/?network=testnet&simplePriceFeeds=1. Note the URL parameters are necessary for running on a local testnet.