# Specification of LAP

## Values to be used
Following values except CONTENT are all represented as String. 

### CONTENT
Byte stream to be approved, such as file stream.

### INDEX
Non-negative integer starting with 0 indicating the order in which users append each record.

### DATETIME
Represented as the extended format defined in ISO 8601. In this specification, values cannot omit any time element nor
use fraction. For example, '2020-01-01T09:23:00Z' and '2020-12-31T23:59:59+0900' are allowed.
'2020-01-01T09:00Z' and '2020-12-31T23:59:59.11+0900' are not allowed.
Usually, this value indicates when an approver approved.
The manager can, however, give another meaning this value, for example when an approver received a message. 

Remember users can set this value arbitrarily.

### TITLE
Usually, this value names CONTENT or what an approver does. For Example, 'Request for using the company car' 
or 'Receive Document'.

### APPROVER
this value is used for fetching their public key. this value is unique to its key.  

### HASH
a value produced by applying a certain hash function to a set of above values. This value is used for checking 
CONTENT's integrity.

### SIGNATURE
a value produced by applying a certain signing algorithm to a set of HASH and tha last SIGNATURE.  

## Publish
In this specification, *Publish* means to expose data so that every stakeholder can see the data and check 
its being not faked. Where to publish depends on the level of security the manager needs. It varies such as
bulletin board, newspaper, SNS or National Archives. If the manager trust their system and users,
putting database in a shared folder would be enough.
Even if this case, users cannot fake others' signatures unless their private keys are not stolen.

## Protocol

### Create database
The manager must create the place where each record should be written down. Usually it would kind of SQL database 
or CSV file. Also, the manager must publish what kind of function or algorithm is used and a list of approvers. 
The procedures of function and algorithm should be noted as explicitly and minutely as possible so that users can validate 
each record from published data when the system get broken. Never forget character encoding.

### Add record
Calculate HASH from (CONTENT, INDEX, DATETIME, TITLE, APPROVER). Then, calculate SIGNATURE from 
(HASH, the last SIGNATURE) with the approver's private key. Finally, append
(INDEX, DATETIME, TITLE, APPROVER, HASH, SIGNATURE) into the database.

### Publish database
Ideally, it is the best way to publish (or republish) the whole database immediately after a record has been appended. 
If it is difficult for some reasons, publish part of database once in while.

### Publish change of database specification.
If some changes occurs in database specification like hash function or signing algorithm,
the manager must publish them immediately.
They also include registration or deletion of approver. 
