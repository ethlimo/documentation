# Filebase IPFS Pinning

### What is IPFS Pinning? <a href="#what-is-ipfs-pinning" id="what-is-ipfs-pinning"></a>

IPFS pinning refers to the process of specifying data to be retained and persist on one or more IPFS nodes. Pinning assures that data is accessible indefinitely, and will not be removed during the IPFS garbage collection process.

#### Understanding the Garbage Collection Process <a href="#understanding-the-garbage-collection-process" id="understanding-the-garbage-collection-process"></a>

When files and data are stored on the IPFS network, nodes on the network cache the files that they download and keep those files available for other nodes on the network. Since storage on these nodes is finite, the cache for each node must be cleared periodically to make room for new files to be cached and made available. The process of clearing the cache for IPFS nodes is referred to as the IPFS garbage collection process.

Garbage collection is an automatic process that is used to manage resources, such as IPFS node disk space. The process is designed to remove cached data that it thinks is no longer needed, though if your IPFS CID refers to a file that is vital to your workflow or environment, having this file removed can be detrimental. So how do you prevent your file from being removed during this process?

#### IPFS Pinning <a href="#ipfs-pinning" id="ipfs-pinning"></a>

To protect data from the garbage collection process, data must be pinned on the IPFS network. This ensures that data is retained indefinitely and is always accessible. Pinning is useful for a variety of workflows, such as accessing data files from around the world without managing sharing permissions. All data uploaded to IPFS is public by default since all you need to access it is the file’s CID. There are no permissions, user accounts, or other security settings tied to IPFS CIDs.

One of the most popular workflows utilizing IPFS pinning right now are NFT collections. These collections have a variety of files involved, including NFT image files and their associated metadata files. If these files aren’t pinned on IPFS and they get removed by the IPFS garbage collection process, this can result in an “NFT Rug Pull”, which means that the NFT ceases to exist, and is no longer accessible or transferable.

#### Pinning Services <a href="#pinning-services" id="pinning-services"></a>

IPFS pinning can be configured on locally hosted IPFS nodes, but for external, long-term storage, that’s where pinning services such as Filebase come in.

### How To Pin New Data on IPFS With Filebase <a href="#how-to-pin-new-data-on-ipfs-with-filebase" id="how-to-pin-new-data-on-ipfs-with-filebase"></a>

Files uploaded to an IPFS bucket on Filebase are _automatically_ pinned to IPFS and stored with **3x** **replication** across the Filebase infrastructure by default, at no extra cost to you. This means your data is accessible and reliable in the event of a disaster or outage, and won't be affected by the IPFS garbage collection process.

#### Uploading a File to IPFS Through The Filebase Web Dashboard <a href="#uploading-a-file-to-ipfs-through-the-filebase-web-dashboard" id="uploading-a-file-to-ipfs-through-the-filebase-web-dashboard"></a>

1\. Start by clicking on the ‘Buckets’ option from the menu to open the Buckets dashboard.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FOPmPz12qis80U53ofzks%252FFilebase___Decentralized_Storage_Made_Easy.png%3Falt%3Dmedia%26token%3D466cecae-ac7f-4a57-b467-68d3a0c1b2af&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=4be207adb071e913ac8b7882dfd8b3a057d0fad36d102c7616540eab802e60c4" alt=""><figcaption></figcaption></figure>

2\. Select your IPFS Bucket.

3\. After clicking on the bucket name, you will see any previously uploaded files. To upload another file, select 'Upload', then select 'File' from the options.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FqFGgqyCetcSx9mdL892q%252FFilebase___Decentralized_Storage_Made_Easy.png%3Falt%3Dmedia%26token%3Db197232a-9034-4947-b012-7420ba20d780&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=948a10819b227824e963b8b80686ff6d35c51beeed613156fcaddc2666c6b144" alt=""><figcaption></figcaption></figure>

4\. Select the file you want to upload to the IPFS.

5\. Once uploaded, you will be able to view and copy the IPFS CID from the 'CID' category, as seen below.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252F9ofAK5zugVY1TQsbNE6K%252FFilebase___Decentralized_Storage_Made_Easy.png%3Falt%3Dmedia%26token%3D4420bac7-30b5-4b90-b930-27108b6fae9d&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=9d5ff6c14d17587d43cad8be76c2b22c8ffb987c2f26e066b19acefeb923b9a1" alt=""><figcaption></figcaption></figure>

#### Uploading a Folder to IPFS Through The Filebase Web Console <a href="#uploading-a-folder-to-ipfs-through-the-filebase-web-console" id="uploading-a-folder-to-ipfs-through-the-filebase-web-console"></a>

1\. Start by clicking on the ‘Buckets’ option from the menu to open the Buckets dashboard.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FOPmPz12qis80U53ofzks%252FFilebase___Decentralized_Storage_Made_Easy.png%3Falt%3Dmedia%26token%3D466cecae-ac7f-4a57-b467-68d3a0c1b2af&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=4be207adb071e913ac8b7882dfd8b3a057d0fad36d102c7616540eab802e60c4" alt=""><figcaption></figcaption></figure>

2\. Select your IPFS Bucket.

3\. After clicking on the bucket name, you will see any previously uploaded files. To upload another file, select 'Upload', then select 'Folder' from the options.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FihyJw4JoOmlnnwknZ3Bp%252FFilebase___Decentralized_Storage_Made_Easy.png%3Falt%3Dmedia%26token%3Dd4dd8677-1239-425c-a814-91bba40ca32a&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=01e2bdf6df6cb2975fc8d1860c60938b993976fc4ca82b624ea740285ef510b1" alt=""><figcaption></figcaption></figure>

4\. Select the folder you'd like to upload to IPFS.

5\. Once uploaded, the folder will look similar to IPFS individual files.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252Fic587flpBOeqbsH41bev%252FFilebase___Decentralized_Storage_Made_Easy.png%3Falt%3Dmedia%26token%3Db50e1c96-2d97-403f-8cc8-b144ccad9274&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=463085f90bd8ade02913f78094313a68b292c7263d97e5f009ff65c12582ee95" alt=""><figcaption></figcaption></figure>

6\. Copy the IPFS CID for your folder, then navigate to `https://ipfs.filebase.io/ipfs/[CID]`. The contents of your folder will be listed.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FeoOwDyMm9Vi4MtN7TgbH%252F_ipfs_QmbDerU94JSSzVbqmyrsEkFam3GQpjSDa9Wm1qnaB27t2Z.png%3Falt%3Dmedia%26token%3Dc5adc1cc-2337-475c-884b-91a1d513b327&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=ef381d8760261240328c9a9473fc1830da5bd98ee5a7b30fe5fd80104e4e6319" alt=""><figcaption></figcaption></figure>

#### Uploading a File to IPFS Using the S3-Compatible API <a href="#uploading-a-file-to-ipfs-using-the-s3-compatible-api" id="uploading-a-file-to-ipfs-using-the-s3-compatible-api"></a>

If you're using the S3 compatible API, the CID will be returned in the response of a PutObject call.

For example, if we run the following AWS CLI command:

`aws --endpoint https://s3.filebase.com s3 cp test-images/7FIMFhlMf6A.jpg s3://ipfs-test --debug`

For more information on AWS CLI, see [here.](https://docs.filebase.com/third-party-tools-and-clients/cli-tools/aws-cli)

The response is shown below. For convenience, we've highlighted the respective response header:

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FwlSrtLrgnF8jsBcKa0AO%252Fimage.png%3Falt%3Dmedia%26token%3D91af3c4c-e46d-4ce4-85d4-5a1c6b83e0d0&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=02142e5eee26d4cdabb010b92fbb04d6bfcd152b42dd3b0e379ae3d669aa51c7" alt=""><figcaption></figcaption></figure>

You can also call the HeadObject API to fetch the CID at any time as well:

`aws --endpoint https://s3.filebase.com s3api head-object --bucket ipfs-test --key 7FIMFhlMf6A.jpg`

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252F5w5ICRiuYWPZDLQWP5Tp%252Fimage.png%3Falt%3Dmedia%26token%3D394cc51f-75c0-41c3-9868-cf55cad6eac8&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=1969439b7ebc9f11664ec904bc3164cc331c1f8d89f3e1838816808228b66923" alt=""><figcaption></figcaption></figure>

#### Uploading a Folder to IPFS Using IPFS Desktop then Pinning it Using Filebase <a href="#uploading-a-folder-to-ipfs-using-ipfs-desktop-then-pinning-it-using-filebase" id="uploading-a-folder-to-ipfs-using-ipfs-desktop-then-pinning-it-using-filebase"></a>

1. Start by downloading the [IPFS Desktop GUI client](https://github.com/ipfs/ipfs-desktop/releases), or navigating to the [IPFS Web UI.](https://github.com/ipfs/ipfs-webui)
2. Select 'Files' from the left side bar menu.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FXweIVXPihfMe4P2GoyO7%252FScreen_Shot_2023-02-09_at_5_10_41_PM.png%3Falt%3Dmedia%26token%3Df44be514-b85f-4657-96bc-ab1017f0d073&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=a792728dd348017a57b686383efdb15618b493e5f10a332de10a64b3d3f86def" alt=""><figcaption></figcaption></figure>

1. Select 'Import'

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252F5deSFfonf45VkqjjwF8g%252FScreen_Shot_2023-02-09_at_5_10_41_PM.png%3Falt%3Dmedia%26token%3D739c36c7-8dec-443c-bb01-e1798ea268b1&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=28a38fb32d8439c59b436b67300f2a18cbf7769ac3e6209ce7cb316bdcba3587" alt=""><figcaption></figcaption></figure>

1. Select 'Folder' from the list of options.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FkMK0fmOPHsK00yDUObIH%252FScreen_Shot_2023-02-09_at_5_10_48_PM.png%3Falt%3Dmedia%26token%3D87a5738d-887f-4a4b-bcb2-9a50155d11ac&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=6f9d379e743b956ea4f3ab900d6a13b16e8f0271f5ad8222c9cb0d9706f0fffa" alt=""><figcaption></figcaption></figure>

1. Select the folder you'd like to upload. Once it has been imported into IPFS Desktop, select the three dots to the right of the screen.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FXG48UR4VokELS1daWSef%252FScreen_Shot_2023-02-09_at_5_11_14_PM.png%3Falt%3Dmedia%26token%3Dfd9c5e7e-7180-4f86-87b7-9604fdab5da2&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=3f03193726a79390effc5ee79e1d27992edac1ca120c0fd5d0eb55c62264467f" alt=""><figcaption></figcaption></figure>

1. From the list of options, select 'Copy CID'.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FIkXxryFhFkrT0PhJo6Lx%252FScreen_Shot_2023-02-09_at_5_11_26_PM.png%3Falt%3Dmedia%26token%3D08765e72-6953-4fd0-8cf5-ffc195783424&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=575c6cd7bb1fdb1f7ec5da71308108ab4caf1dfbd969837f90f9245a7dd93e90" alt=""><figcaption></figcaption></figure>

1. Next, navigate to the [Filebase Web Console dashboard](https://console.filebase.com/). Click on the ‘Buckets’ option from the menu to open the Buckets dashboard.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FxLGj2qFG1UH6zlcrGAYa%252F1.png%3Falt%3Dmedia%26token%3D69844a9b-a2b5-4a72-af8f-59157d5c5dde&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=9eb7c8b24514ba6cebc9892181bd791d8d571b918873079504b03828e7e9ec86" alt=""><figcaption></figcaption></figure>

8\. Once at the Buckets dashboard, create a new IPFS bucket by clicking the ‘Create Bucket’ option in the top right corner.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252Fj1Uyqbq21NCgdt1S78Zu%252F2.png%3Falt%3Dmedia%26token%3Ddcae939d-740f-40a7-96ab-59e68a330666&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=31abb4316dedeac86f33d2df2857497dcf6954af0a0221c904466686c3b252ea" alt=""><figcaption></figcaption></figure>

9\. Enter a bucket name and choose the IPFS network.

_Bucket names must be unique across all Filebase users, be between 3 and 63 characters long, and can contain only lowercase characters, numbers, and dashes._

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252F15jJc8wlMDJFC9Hsjk6X%252Fipfs-bucket.png%3Falt%3Dmedia%26token%3Dfdd6b205-2379-4708-8439-6058c7d60fa8&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=861ef655216afd9e831c0350f3b785adc16fa141e644b7747042a83e5d9d4956" alt=""><figcaption></figcaption></figure>

10\. Then select your new IPFS Bucket.

11\. After clicking on the bucket name, select 'Upload' from the top right corner, then select 'CID'.

Pin by CID is a paid feature that requires a paid Filebase IPFS subscription plan.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252FlPnHWiG5OUM1aoc0BExz%252FPasted_Image_7_19_22__3_18_PM.png%3Falt%3Dmedia%26token%3Df257b951-c45d-491e-ab1a-cde3d81de510&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=15be35d82fbd58f2a53916feed05b5df23db5d734f20efd42bc2bdd4f830c00d" alt=""><figcaption></figcaption></figure>

12\. Then enter your IPFS CID that you copied from IPFS Desktop and a custom human-readable name to associate with your CID.

<figure><img src="https://docs.filebase.com/~gitbook/image?url=https%3A%2F%2F3861818989-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-Lyjw7dWpiQtUFDa1pO0%252Fuploads%252F5HjZQXpP8ZqV3BIyTggW%252Fimage.png%3Falt%3Dmedia%26token%3D13ac8d9d-7e14-4aa7-91ca-5a21ba090762&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=9dd6e2072a907af27cdd45e7a7f78a4d8cea125fe29297fc70b3ff3faa89463a" alt=""><figcaption></figcaption></figure>

13\. Select 'Search and Pin' to pin your CID to IPFS through Filebase.

**Note:** The IPFS network is large and it may take some time for Filebase's IPFS nodes to locate and fetch the CID.
