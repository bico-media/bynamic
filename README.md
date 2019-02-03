Status: draft

# Dynamic data from a static blockchain

> Lets get a common way of keeping same link to a transaction while updating the content that is being presented: Keep the link - update the content. 

This document contains the description for the protocol B://ynamic (pronounced bynamic). Please share [inputs and comments](https://github.com/bico-media/bynamic/issues) on Github.

## High-level draft

Transactions on the blockchain that includes the bitcom namespace of `1LnUj9stftVnfMD6yJv2qvZZKLh2iTsmZs` shall be called a bynamic transactions. 

### v1

_External reference_

A bynamic transaction v1 shall have 1 argument to the `1LnUj9stftVnfMD6yJv2qvZZKLh2iTsmZs` bitcom namespace: A transaction ID called `target tx`. The namespace and `target tx` must be the 1st and 2nd field after an `OP_RETURN` code.

If you provide content from the blockchain you are compatible with the B://ynamic protocol v1 if 

- A client requesting the content of a bynamic transaction v1 will receive the content from the `target tx` of the most recent bynamic transaction from same sender as the transaction requested by the client. 


### v2

_Internal reference_

A bynamic transaction v2 shall have 1 argument to the `1LnUj9stftVnfMD6yJv2qvZZKLh2iTsmZs` bitcom namespace: A public bitcoin address called `presenter`. It must be part of a transaction that by itself includes content. 

If you provide content from the blockchain you are compatible with the B://ynamic protocol v2 if you: 

- Are compatible with the B://ynamic protocol v1

- A client requesting the content of a bynamic transaction v2 will receive the content from the `target tx` of the most recent bynamic transaction v1 signed by `presenter` or - if no one exists - the content of the transaction requested by the client. 



### v3

_Group external references_

A bynamic transaction v3 shall have 2 arguments to the `1??????????????` bitcom namespace. The namespace is located at the 1st position after an `OP_RETURN` code. The first argument is a string called `key` and the second argument is called a `target tx`.

If you provide content from the blockchain you are compatible with the B://ynamic protocol v3 if you:

- Are compatible with the B://ynamic protocol v2

- A client requesting the content of a bynamic transaction v3 will receive the content from the `target tx` of the most recent bynamic transaction v3-A having the same sender and `key` as the transaction requested by the client. 



### v4

_Group internal reference_

The namespace is not located at the 1st position after an `OP_RETURN` code. The first argument is a string called `key` and the second argument is a bitcoin address called `presenter`.

If you provide content from the blockchain you are compatible with the B://ynamic protocol v4 if you: 

- Are compatible with the B://ynamic protocol v3

- A client requesting the content of a bynamic transaction v4 will receive the content fom the `target tx` of the most recent bynamic transaction v3 with `presenter` as sender and having same `key` as the current transaction.  


## Referencing bynamic content

In theory bynamic content can be requested the same way as any other content on the blockchain: by using the hexadecimal version of the transaction ID. Example: 

    B://66a6e1eb5ebecca707a03740069461a6b8fa9cca3753d00009f49d1d68a15d25

If sharing bynamic content like this you (as a content creator) cannot know what will be displayed as your audience might not use a gateway to bitcoin content that supports any version of the B://ynamic protocol. Referencing a v2 or v4 transaction would always give the original content instead of the latest version. To avoid this confusion bynamic content must be referred to with the _raw_ transaction ID data encoded with z-base-32 encoding - now referred to as a `tx32`. The [z-base-32 encoding](https://philzimmermann.com/docs/human-oriented-base-32-encoding.txt) (Also known as Zooko encoding) has been selected because it does not include padding and seeks to please the eye better by using common letters the more often. Please note that this is **not** the [RFC 4648 base32](https://tools.ietf.org/html/rfc4648). 

Example of a tx32 version of the previous example: 

    B://c4uqd446z5gkqb7yg7yypfdbw4hxi8gkg7j7yyyj61qt44fbmw1o

Same transaction ID but different representation. Any string that matches `/^[a-km-uw-z13-9]{52}$/i` is a valid tx32. It will always be 52 chars long. Content providers supporting any version of the B://ynamic protocol will be able to process these the same way as any other regular reference while providers not supporting the protocol will ignore the reference as they do not match a valid hexadecimal transaction ID.

Please note that any transaction IDs on the blockchain is still in the original hexadecimal format. The tx32 notation is **only** for when referencing bynamic content from outside of the blockchain. 

----

Please share [inputs and comments](https://github.com/bico-media/bynamic/issues).
