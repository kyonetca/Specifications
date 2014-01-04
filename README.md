#DCCC (DC3, Decentralized Cryptic Chat)
Decentralized Cryptic Chat, short DCCC, short DC3 is a decentralized Messenger. It's architecture is similar to well known services like BitCoin etc. All communication is being encrypted via [NaCl](http://nacl.cr.yp.to/) because it's awesome and open source and portable and yeah you know it. Asymmetric Encryption is great.

##Architecture
There are two Systems: The Servers and the Clients. The Clients are lateron used by the users. The servers provide the backend. The amount of servers is theoretically endless. Everyone can run a server and is encouraged to do so. The server communicate with each other to exchange the users public-keys and other information.

###Servers
Each server stores a list of the other servers it already knows so other servers can easily discover those and servers can send messages to other servers. Also, every server saves user-info-records. The values those contain are defined under the specific Section of this document. 

###Clients
The Clients are just mobiles. The operating system doesn't matter as long as it provides everything necessary to implement a client. A client can open a socket to an arbitrary server which is in the network.

##Crypto
Encryption is a really important part of DC3. Because of that, all messages, both between servers and between servers and clients are being encrypted. For that, NaCl should be used (you could use any other implementation of those algorithms but NaCl is pretty cool). To be exact, NaCl is using Curve25519 for scalar multiplication, Salsa20 for secret-key authenticated encryption and secret-key encryption and Poly1305 for one-time authentication. ([Source](http://nacl.cr.yp.to/valid.html) 4.1.14).

##Communication
All communication is encrypted. See the Crypto-section for that. The Communication is happening via Sockets. Clients should connect to servers on port 42961. Servers communicate with each other on port 42962. Messages are JSON-Encoded.

###Server <-> Client
Messages look like this:

```
{
	"type": <the type of the message, as a string>,
	<other properties specific for the message>
}
```

When communicating, each message is one line long. Messages are delimited by a line-break ("\n").

There are several message-types:
####From the Client:

#####"REG": register the user
**Parameters:**

* **phoneNumber:** the phone number of the user. TODO: verify this?
* **publicKey:** the publickey of the user

#####"GPK": get public key
**Parameters:**

* **of:** SHA-256'd mobile-number of the person you want the public key from

#####"STM": send text message 
**Parameters:**

* **to:** SHA-256'd mobile-number of your partner
* **content:** Base64 encoded, NaCl encrypted message
* **id:** A random id generated by the client to lateron identify this message

#####"ACK": acknowledge a message
**Parameters:**

* **to:** SHA-256'd mobile-number of your partner
* **id:** The id of the message to be acknowledged

*TBC*

####From the Server 

#####"PPK": partners public key
**Parameters:**

* **partner:** SHA-256'd mobile-number of the public key's owner
* **key:** Base64 encoded publickey

#####"GTM": got text message
**Parameters:**

* **from:** SHA-256'd mobile-number of the sender
* **content:** Base64 encoded, NaCl encrypted message
* **id:** A message id generated by the sender

#####"ACK": partner acknowledged a message
**Parameters:**

* **from:** SHA-256'd mobile-number of your partner
* **id:** The id of the acknowledged message

*TBC*


###Server <-> Server
*TBC*
