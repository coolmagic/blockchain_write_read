# Simple Step by Step Process to Show how to Write a Message on Block Chain (Part of transaction) & Read it

***** You should have Local blockchain running

Let say you have an address with UNSPENT money (50 BTC) in this example:
```
➜  bitcoin-cli -regtest listunspent
[
  {
    "txid": "125c47354b1a01c766fa0d947a4a3c220fb3df40465f62725ec17e0a1483cad3",
    "vout": 0,
    "address": "msBueFWW8qX4hvyU5cygGRg1xD6ocSdwfn",
    "scriptPubKey": "21024e6f648bf2b6cef7e0603eb2bcf78ab9bcc4e40b5c3cb6c9ef3abc6c41ff8a2eac",
    "amount": 50.00000000,
    "confirmations": 101,
    "spendable": true,
    "solvable": true,
    "safe": true
  }
]
```
-----------------

* You need to create a RAW transaction where 'data' attribute of transaction can hold our string "Hello World - VJV" in HEX form. 'VJV' is suffix to identify the string.
I converted the string to HEX from https://www.asciitohex.com/. The HEX of the String is: 48656c6c6f20576f726c64202d20564a56

* Now create the transaction using followinf command:
```
➜  bitcoin-cli -regtest createrawtransaction '[{"txid":"125c47354b1a01c766fa0d947a4a3c220fb3df40465f62725ec17e0a1483cad3","vout":0}]' '{"data":"48656c6c6f20576f726c64202d20564a56","msBueFWW8qX4hvyU5cygGRg1xD6ocSdwfn":49.90}'

0200000001d3ca83140a7ec15e72625f4640dfb30f223c4a7a940dfa66c7011a4b35475c120000000000ffffffff020000000000000000136a1148656c6c6f20576f726c64202d20564a56805b6d29010000001976a9148007647d7877e947e71800dddea814117df7a42c88ac00000000
```
So we have TX Hash now.

* Next step is to SIGN the transaction
```
➜  bitcoin-cli -regtest signrawtransaction "0200000001d3ca83140a7ec15e72625f4640dfb30f223c4a7a940dfa66c7011a4b35475c120000000000ffffffff020000000000000000136a1148656c6c6f20576f726c64202d20564a56805b6d29010000001976a9148007647d7877e947e71800dddea814117df7a42c88ac00000000"
{
  "hex": "0200000001d3ca83140a7ec15e72625f4640dfb30f223c4a7a940dfa66c7011a4b35475c120000000049483045022100e68a4d4921f6094d9f4ed7918ca7ca1191662d8259d8ffa52e42b3d8f632e03f02200275dc037e26c8f1387d25412e1e11b08e473840e5a8d0bbf26ae142117012d001ffffffff020000000000000000136a1148656c6c6f20576f726c64202d20564a56805b6d29010000001976a9148007647d7877e947e71800dddea814117df7a42c88ac00000000",
  "complete": true
}
```

* And finally we broadcast the transaction:
```
➜ bitcoin-cli -regtest sendrawtransaction "0200000001d3ca83140a7ec15e72625f4640dfb30f223c4a7a940dfa66c7011a4b35475c120000000049483045022100e68a4d4921f6094d9f4ed7918ca7ca1191662d8259d8ffa52e42b3d8f632e03f02200275dc037e26c8f1387d25412e1e11b08e473840e5a8d0bbf26ae142117012d001ffffffff020000000000000000136a1148656c6c6f20576f726c64202d20564a56805b6d29010000001976a9148007647d7877e947e71800dddea814117df7a42c88ac00000000"

```

908ea777cab9d0b3537c65bf2447418f98ab2c3c189a95e6f57f3143e0837998    <----- This is transaction ID. We can see on any block chain explorer.
```
➜ bitcoin-cli -regtest getrawtransaction "908ea777cab9d0b3537c65bf2447418f98ab2c3c189a95e6f57f3143e0837998" > output.txt
```
File output.txt will have the output. This attribute will have this message:
```
"asm": "OP_RETURN 48656c6c6f20576f726c64202d20564a56"
```

-- Here is the complete RAW transaction

```
{
  "txid": "908ea777cab9d0b3537c65bf2447418f98ab2c3c189a95e6f57f3143e0837998",
  "hash": "908ea777cab9d0b3537c65bf2447418f98ab2c3c189a95e6f57f3143e0837998",
  "version": 2,
  "size": 186,
  "vsize": 186,
  "locktime": 0,
  "vin": [
    {
      "txid": "125c47354b1a01c766fa0d947a4a3c220fb3df40465f62725ec17e0a1483cad3",
      "vout": 0,
      "scriptSig": {
        "asm": "3045022100e68a4d4921f6094d9f4ed7918ca7ca1191662d8259d8ffa52e42b3d8f632e03f02200275dc037e26c8f1387d25412e1e11b08e473840e5a8d0bbf26ae142117012d0[ALL]",
        "hex": "483045022100e68a4d4921f6094d9f4ed7918ca7ca1191662d8259d8ffa52e42b3d8f632e03f02200275dc037e26c8f1387d25412e1e11b08e473840e5a8d0bbf26ae142117012d001"
      },
      "sequence": 4294967295
    }
  ],
  "vout": [
    {
      "value": 0.00000000,
      "n": 0,
      "scriptPubKey": {
        "asm": "OP_RETURN 48656c6c6f20576f726c64202d20564a56",
        "hex": "6a1148656c6c6f20576f726c64202d20564a56",
        "type": "nulldata"
      }
    },
    {
      "value": 49.90000000,
      "n": 1,
      "scriptPubKey": {
        "asm": "OP_DUP OP_HASH160 8007647d7877e947e71800dddea814117df7a42c OP_EQUALVERIFY OP_CHECKSIG",
        "hex": "76a9148007647d7877e947e71800dddea814117df7a42c88ac",
        "reqSigs": 1,
        "type": "pubkeyhash",
        "addresses": [
          "msBueFWW8qX4hvyU5cygGRg1xD6ocSdwfn"
        ]
      }
    }
  ],
  "hex": "0200000001d3ca83140a7ec15e72625f4640dfb30f223c4a7a940dfa66c7011a4b35475c120000000049483045022100e68a4d4921f6094d9f4ed7918ca7ca1191662d8259d8ffa52e42b3d8f632e03f02200275dc037e26c8f1387d25412e1e11b08e473840e5a8d0bbf26ae142117012d001ffffffff020000000000000000136a1148656c6c6f20576f726c64202d20564a56805b6d29010000001976a9148007647d7877e947e71800dddea814117df7a42c88ac00000000",
  "blockhash": "5d8d85769a18c2a16f46ea5cf727624c90aa43fab6c6cbb9b4fd353756b5abbc",
  "confirmations": 101,
  "time": 1525189767,
  "blocktime": 1525189767
}
```
