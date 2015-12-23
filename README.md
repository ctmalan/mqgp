# mqgp
MQTT PGP securtity module
=========================

Introduction
------------

This is an application layer effort to implement a workable security model for mqtt.

The aim of this model is to be:

- simple
- based on good securty principals
- open
- not to be obscure
- based on data ownership (multiple owners might be possible)
 
It is the intent to implement the security at gateway level althoug the model can be implemented on any topic.

A (fail)safe mechanism is envisaged to to change the security model. 

Summary
-------

Each secure topic will have a subtopic called mqgp.conf. The mqgp.conf topic will contain the mqgp.conf configuration file for the topic tree.

The mqgp.conf file will contain the following main entries to allow the gateway to do a complete configuration:

- the server URL
- optional username
- optional password algorithm (DH key exchange etc.)
- optional network security setup to use
- the topic (if the topic is encrypted, this will be the encrypted form)
- the owner of the topic (single ownership only at the moment)
- optional the encryption algoritm used for the contents and further information, such as the gateway public key

The mechanism is as follows:

1. gateway will publish unprotected mqgp.conf on an unprotected topic not using any security mechanisms
2. the gateway will subscribe to secure mqgp.conf published using the full security specified in mqgp.conf
3. if secure mqgp.conf exists, the gateway publishes the rest of the topic tree using the security as specified in mqgp.conf
4. the owner can publish any updates using the full security layers to secure mqgp.conf
5. the gateway will publish the unsecure copy of the revised mqgp.conf and subscribe to the revised secure copy of mqgp.conf
6. if the new copy of secure mqgp.conf exists the gateway will use the revised mqgp.conf as new configuration to publish the data


The unsecure version of mqgp.conf is required for information only and is not used for configuration. The gateway subscribes to the secure version and only publishes secure information if the subscription is successful.

The source of the unsecure version is the mqgp.conf file on the gateway. The mqgp.conf file on the gateway is only updated if a copy of the new secure mqgp.conf is available using the new mqgp.conf file. The gateway will then publish the new mqgp.conf and use it.

To change ownership of the gateway, the old owner will transfer ownership by providing a new mqgp.conf.  The new owner will take take ownerhip by publishing the same file.

This concludes the conceptual design and the detail design will follow.

c-:



