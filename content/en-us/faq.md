---
title: "Faq"
date: 2023-07-10T12:14:12-04:00
draft: true
---
# Frequently Asked Questions

## What is a repository?

A _**repository**_ is simply a place where you can store and share your files and folders securely, using **Ouisync**. You can think of it as a root folder, or even a safe, that will contain other folders and files that you want to share with your peers.

## Where can I see my repositories?

When you open the **Ouisync app**, after the onboarding screens, you will see the main screen listing all the repositories you have created. Initially this screen will be empty, but as you go creating _**repositories**_, they will be listed here. 

(insert screenshot)

## How can I create new repositories?

To create a new _**repository**_, tap the  **+** button, (Insert the screenshot of the button) then select **"Create Repository"**. The app will ask you to add a local password -It is not mandatory and you can also add it later. 

#### See [**_What is a 'local password'?_**](https://github.com/equalitie/ouisync-app/wiki/_new#what-is-a-local-password) to learn more about them.

(insert the screenshot of the selection)

## How can I add files and folders to my repository?

That's easy. Go to the _**repository**_ contents screen (insert screenshot) and tap on the **+** button. This will open a small window where you can choose whether to create a folder for your files within that repository or add files to it from your device or external storage (such as a USB stick or SD card).    

## What does it mean to **'import'** a repository?

To import a _**repository**_ means that you want to recreate on your device a _**repository**_ that a peer has shared with you.  

You start with the same **+** button and then select **'Import'** (Insert the screenshot of the Import button); this action will import into your device all the files contained in the _**repository**_ that your peer shared with you.

## How can I share my _**repository**_ with my peers (or my other devices)?

You can do this by tapping the _**repository**_ settings symbol (insert the symbol) and then tap on Share symbol (insert symbol). 

If the peer (or device) with whom you want to share a _**repository**_ is nearby, they can tap on **'Import repository'** on their device (insert screenshot), and then scan the QR code displayed on your screen (insert QR screenshot of QR code). 

This action will import a copy of your _**repository**_ onto your peer's device, including all the files and folders within it. 

If your peer is not nearby, you can instead generate a token link (insert screenshot) which you can send to them via email, any messaging application, etc. They will need to copy and paste that link into the field provided when they tap **'Import Repository'** on their device. (insert screenshot)

**PS:** to paste a link to the input field, you tap and hold your finger on it, until a small **Paste** button appears (insert Paste button); then you tap that... (you probably already knew that... but this is in case you didn't...)

## How do I decide which permissions to select when sharing a _**repository**_?

### **Read / Write**

It depends what you want the peers with whom you shared your repository to be able to do. If you want them to be able to add files, delete them, rename or move them, then you share your repository with **Read / Write permissions**.

An example of a use case for this level of permissions: _sharing photos with friends and family, or working collaboratively on a project._ 

### **Read**

If you want your peers to only be able to read the _**repository**_ contents, then you select... (you guessed it... ) **Read permissions**. This means they will be able to open the files and read them, but they won't be able to add new files to your shared _**repository**_, or delete any files from it, or move them, etc.

An example use case would be _when you want to share the information regarding an event, or news items, or maybe regarding certain products, or may be you are a teacher sharing some content with your students, etc. Then you want the recipients to be able to read the contents but not change them._ 

### **Blind**

This level of permissions can be useful when you want to securely store your _**repository**_ as a backup. This means that the person or device with whom you shared your repository as **'blind'** won't be able to either open the files to read them, or make any changes to them. This way you can store your data securely on a friend's computer, for example. 

## How can I use my _**repository**_ as secure backup?

### Create Secure Backup

You can create a secure backup repository on your own or even on a friend's device.

To do that you first need to generate the **'read and write'** token link for the _**repository**_ that you want to store blind. You keep the **'read and write'** token link somewhere safe, as you will need it for retrieving the information from your blind copy later on.

Then you create a **'blind'** token link and import a blind repository into the backup device.

### Retrieving Information from the Blind _**repository**_

If you accidentally delete a _**repository**_ from your primary device, what you can do is go to **'Import _Repository_'**, copy and paste the **READ-WRITE** token that you kept somewhere safe into the provided field, and that's it. Once your primary device connects with your backup device, they will sync - i.e.: the primary _**repository**_ will automatically sync with your backup _**repository**_ and receive all the files that _**repository**_ contains.

#### **Note:** if you add files to your primary _**repository**_, that addition will be propagated to your backup _**repository**_ too (if your backup device is connected/online. That means that your backup _**repository**_ will automatically receive all updates from your primary _**repository**_.

But if you delete any files in your primary _**repository**_, then that deletion will be propagated too, and you won't be able to retrieve those files. So **Ouisync** is currently primarily a synchronization tool and not a secure backup tool.

The selective syncing, and creating snapshots in time that will allow you to go back to the previous version of your _**repository**_ is foreseen for development onfuture **Ouisync** releases.

#### **Notice:** if you lose your **'read-write'** token link for the backup _**repository**_, you won't be able to retrieve data from that blind copy. 

## Can my peers re-share my token links? 

Yes. They can generate the token links with the same permissions they had in the original token link that they received from you, or lower.  

This means that if a person has received a token link to import a _**repository**_ with **'read-write'** permissions they are able to generate the same kind of token to share the same _**repository**_ with other people, or they can also generate the token links for the same _**repository**_ but with lower permissions -read only or blind. 

If they imported a **_repository_** with read permissions only, then they can share it with others as read or blind. And if they imported your _**repository**_ as blind, they can only share it on as blind. 

## What happens if I delete files in my _**repository**_?

File deletion is propagated to all replicas in existence -which means, the same file that you deleted will be automatically deleted in the _**repositories**_ of all the peers with whom you have shared it.

Equally, if your peers delete any files in any of the _**repositories**_ that they have imported from you, their file deletions will be propagated to your device too. It works both ways -i.e.: _**repositories**_ shared  with **'read-write'** permissions will sync with each other, including the file edit, addition or deletion.

## What happens if I rename files in my _**repository**_?

If you rename files in your _**repository**_, the new file name will be propagated to the _**repositories**_ of all the peers that you shared your _**repository**_ with.

## Can I move my files from one _**repository**_ to another?

No. At the moment you can only move files from one folder to another within the same _**repository**_. Moving files from one repository to another is planned for future releases of **Ouisync**.

## Are my files stored on a server? Why?

Yes. They are stored encrypted and are not readable by the server.

The purpose of the server storage is to facilitate file syncing when peers are not online at the same time. If you want to share a _**repository**_ with a peer who is not online at the moment, your _**repository**_ data will be stored encrypted on the server and when your peer comes online and connects either to the server (or to your device) the files from the stored _**repository**_ will sync with the files in your peer's _**repository**_.

## How can I connect to the server?

This happens automatically when you share a _**repository**_ with a peer -you don't need to perform any additional actions.

## Where is this server and who runs it?

(Add info about the country where the servers are and so on)

## What private data does **Ouisync** use/store?

**Ouisync** uses the IP addresses of your devices to be able to connect you with your peers in the peer-to-peer network. We don't store those IP addresses anywhere on our systems. We don't keep any other user data. 

## What happens if me and my peers make changes to a _**repository**_ at the same time?

(this I need to test first to understand how it works in practice)

## Do I need to set up a password (or biometric protection) to protect my _**repositories**_?

That is largely up to you. Whether you protect your _**repositories**_ with passwords or biometrics depends on the sensitivity of the data that you store in **Ouisync** _**repositories**_ and habitual usage of your devices.

For storing and sharing photos of your cat, maybe a password is not necessary. But for storing more sensitive personal data, we recommend passwords (or biometrics) to be set up.

You can have a different password for each repository. It is also possible to have a mixture of password (or biometrics) protected **Ouisync** _**repositories**_ and ones without protection. 

## What is a **'local password'**?

Local password means it is a password set up only for your own device. 

You don't need to share it with your peers. They can set up their own passwords to protect the shared **Ouisync** _**repositories**_ on their own devices.

## How can I lock my _**repositories**_ when I'm not actively using them?

To lock your _**repositories**_ when not actively working on them, you need to tap on this button (insert screenshot).

To unlock them, you tap on the repository name or on this icon (insert screenshot).

If your _**repository**_ is protected by password, you need to enter the password when prompted; therwise, just tap on the **Unlock** button and continue to work on your repository.

# Other FAQs

## Advantages of using **Ouisync** over **Dropbox** (or other similar solutions)?

### Free to use

To be able to share files using **Dropbox**, you need to create a **Dropbox** account. This requires your name, email, credit card. It requires payment. 

**Ouisync** is free and open source software. To share files using **Ouisync**, you only need to install the app. That's it. No payment is required.

### Anonymity

**OuiSync** does not require the creation of user accounts. With **Ouisync**, it is simply a matter of installing the app and using it. All users are completely anonymous.

### **OuiSync** is a **P2P** solution

This means that using **OuiSync** successfully does not depend on any central server anywhere. **Ouisync** makes use of decentralized peer-to-peer networking, which makes it an effective file sharing app even in situations where well-known file sharing servers (such as **Dropbox** or **Google Drive**) are unavailable.

## How can I sync the files with my peers or with my other devices without internet?

In situations with limited internet availability, you will need to make sure some means of connecting to other devices still exists. 

This could be a WiFi signal available to all devices that want to share **Ouisync** _**repositories**_, or it could be intranet, local network or similar technologies. 

Currently _**repositories**_ cannot be shared via Bluetooth, but that feature is planned for future releases.



