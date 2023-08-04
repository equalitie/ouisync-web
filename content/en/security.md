---
title: "Security"
date: 2023-07-19T00:41:15-04:00
---
{{% center padding-top="3rem" padding-bot="1rem"%}}
# Ouisync’s unique approach to guarantee encrypted file sharing
{{% /center %}}

## Encryption in a distributed file sharing system
Ouisync offers users a secure way to share and synchronize data across devices. Due to the distributed (peer-to-peer) nature of Ouisync, where concurrent changes to files and directories are possible, the directory structure of Ouisync is quite elaborate. Whenever two or more users modify a file in a directory concurrently, Ouisync’s architecture guarantees that no information is lost. Moreover, Ouisync also protects the content (files and repositories) and the structure of your file systems by implementing end-to-end encryption. 

When sharing a repository in a specific mode (Write, Read, or Blind) Ouisync generates keys (called “tokens”) that you can share with your peers either as links or as QR codes. Importing a repository using a token gives your device the ability to decrypt its directories and files (except in Blind mode). 

Data is encrypted both _at rest_ (when simply stored) and _in transit_ (during data transfers). Importantly, Ouisync can sync without decrypting, and no device needs to know the decryption key to perform the synchronization. File names, file contents and even file sizes and the structure of directories are hidden from peers who do not possess an encryption key. Therefore, peers who only have Blind access to your repositories can see neither the content of your repositories nor their structure. 

## Which encryption algorithms are used?
* _In transit_, Ouisync uses the [Noise protocol](https://noiseprotocol.org/) framework, in particular the [NNpsk0 pattern](https://noiseprotocol.org/noise.html#pattern-modifiers). This lets Ouisync generate ephemeral keys with a pre-shared key. The pre-shared key in Ouisync is the repository ID. Noise supports mutual and optional authentication, identity hiding, forward secrecy, zero round-trip encryption, and other advanced cryptographic features. 
*	_At rest_, Oyisync encrypts the data using [ChaCha20](https://en.wikipedia.org/wiki/Salsa20#ChaCha_variant). In this case the "Read key" is used as the encryption/decryption symmetric key. The keys are authenticated using Ed25519 signatures, with the "Write key" as the private key. 
*	For _hashing_ Ouisync relies on the [BLAKE3](https://en.wikipedia.org/wiki/BLAKE_(hash_function)#BLAKE3) hash function, which is [considered](https://github.com/BLAKE3-team/BLAKE3-specs/blob/master/blake3.pdf) to be consistently faster across different platforms and input sizes.

## What is a Block? 
Every file and every directory stored in Ouisync is divided into relatively small (e.g. 32KB) blocks of a constant size. Each block has a Block ID (generated via a random number generator) that helps Ouisync identify these blocks. All blocks are stored alongside a file called “locator”. Locator is a kind of “map” that indicates where each block is located with respect to other blocks. However, to not reveal this structure to agents who don’t possess the secret key, locators are not stored directly but are encoded.

_Imagine that you organize a big wedding party, where you invite plenty of guests. Those who have already organized these kinds of events know how hard it is to assign proper seats to all the guests, with respect to their relationships, interests and so on. By the way, you need to also communicate the information to the waiters, who have to be attentive and remember which guest have allergies or dietary preferences. And because your guests are VIP, you don't want to reveal their real names to the waiters, so you invent random pseudonyms and write them on those beautiful seat allocation cards. So, if we stick to this metaphor, the block ID would be a pseudonym written on a card next to your guest’s seat, and the “locator” would be a map of all tables with seats properly allocated._

![image](https://github.com/willow446/willow446.github.io/assets/1790886/06985a87-2dac-49a2-99ae-37725bd8e2ce)


## What is a blob?
A linear set of blocks shall be called a blob. Blobs can represent files and directories. The file blob is the simpler one: it consists of a header containing the file size, permissions and a timestamp. The directory blob represents a list of file names present in a directory, as well as locators pointing to the individual file blobs.

## How is syncing happening?
When you share a repository with your peer, this creates a “replica” of your repository. The repository structure is stored in the so-called “index” files - when peer devices are connecting, they first exchange those indexes. If something has been modified in one of the replicas, Ouisync will download the missing blocks. Ouisync always first downloads directories and only after the files themselves. This helps Ouisync to correctly rebuild your data from blocks without messing it up. In addition, this is done without leaking information to users who have no “read” access to your repositories.

You do not have to worry about conflicts between various replicas: in the backend, syncing is done in such a way as to avoid conflicts and divergences. What you see when you open Ouisync is what we call a “snapshot”: your view of the whole directory tree at a particular moment in time. Each modification of the file system (on your device or on your peers’ devices) results in a new “snapshot.” 
