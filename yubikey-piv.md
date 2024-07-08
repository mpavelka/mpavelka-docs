# Set up Yubikey PIV

Docs

- [PIV Commands](https://docs.yubico.com/software/yubikey/tools/ykman/PIV_Commands.html)

Requires

- [ykman](https://docs.yubico.com/software/yubikey/tools/ykman/)

## Generate new management key

The key must be 24 bytes in hex

```
python3 -c 'import os; k=os.urandom(24); print(k.hex().upper());'
```

## Reset Yubikey PIV

```
# Reset
ykman piv reset

# Set new management key
ykman piv access change-management-key

# Set new PUK
ykman piv access change-puk  # Default PUK code: 12345678

# Set new PIN
ykman piv access change-pin  # Default PIN code: 123456
```

## Generate new authentication credentials (slot 9a)

I'm using RSA2048 because it _just works_ with SSH.

```
ykman piv keys generate --algorithm RSA2048 9a pubkey.pem
ykman piv certificates generate --subject "mpavelka" 9a pubkey.pem

```

## Export public key

```
ykman piv keys export 9a -
```

## Convert PEM to ssh-rsa

```
ykman piv keys export 9a - > pubkey.sshrsa
ssh-keygen -f pubkey.sshrsa -i -m PKCS8
```
