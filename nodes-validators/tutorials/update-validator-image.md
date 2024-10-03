## What is a Validator Image?

A validator image is the image representing your validator. It will be displayed on the validator page.

Example on ORAI Scan: [https://scan.orai.io/validators/oraivaloper1uhcwtfntsvk8gpwfxltesyl4e28aalmq9v9z0x](https://scan.orai.io/validators/oraivaloper1uhcwtfntsvk8gpwfxltesyl4e28aalmq9v9z0x)

Example on Scanium: [https://scanium.io/Oraichain/staking/oraivaloper1uhcwtfntsvk8gpwfxltesyl4e28aalmq9v9z0x](https://scanium.io/Oraichain/staking/oraivaloper1uhcwtfntsvk8gpwfxltesyl4e28aalmq9v9z0x)

## Prepare a Cool Image and Update it Yourself

### On scan.orai.io

1. Go to your validator page.
2. Double-click on the validator image.
3. Select your image.
4. The final step is to approve your request.

### On scanium.io

Scanium fetches the image from your Keybase account. If you don’t have a Keybase account, create one and get your public Keybase ID string (in this example, it’s `2798DB6D73F30696`).

If you haven't updated your identity value yet, create a transaction to update your validator info:

```bash
oraid tx staking edit-validator \
--identity "your-keybase-id" \
--chain-id Oraichain \
--from wallet \
--gas-adjustment 1.4 \
--gas auto \
--gas-prices 0.01orai \
-y
```

The final step is to change your Keybase account avatar to the image you want.