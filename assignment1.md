# This my assignment 1

All levels (1-3) are done with refernces provided at the bottom.

## Question

- Create SSH keys on your local machine.
- Add a custom Arch Linux image using the web console.
- Installing and configuring `doctl` on Linux
- Create a new Droplet running Arch Linux using the DigitalOcean web console.
- Use a cloud-init configuration file to automate initial setup tasks (e.g., user creation).
- Create a Droplet running Arch Linux using the `doctl` command-line tool.
- Connect to your server using your SSH keys.
- Use `doctl` and cloud-init for every stage of setting up an Arch Linux droplet

## SSH key

SSH stands for Secure Shell. It is a protocol used to send commands to a computer over an unsecured network in a secure manner. The connection works in a client-server model. The method of securing the connection is done through the method of cryptography. It is a 2 step process:

1. Authentication: Verify the identity of the SSH server.
2. Encryption: A strong symetric encryption algorithm is used, which is transferred over the network using the standard hash algorithms.

When we make a SSH key, we are in reality making a pair of keys. They are the public and private SSH keys. They serve a simple purpose of authenticating the client. The cryptographic pair of keys are created on the client's device, and once created, the public key is shared with the server. Now the keys will act as passwords, so when the client wants to log in, the server will compare the public key to the private key on the client's end. If they match, the connection will be established.

**What's the benifit?** The SSH protocol is an extremely useful tool, as it allows the client (user i.e., you) to access a server remotly. All you need to do is configure an SSH to your server, and you can now work from anywhere (as long as the server is running). *How do you make one?* Following are the steps to creating a SSH key on windows, macOS, and linux.

### How to create an SSH key pair?

To create the key, we will be using the `ssh-keygen` util.
> *Note* on windows, you will first have to create a directory named ".ssh" in your home directory.
The command to create a SSH key pair is:

    ssh-keygen -t ed25519 -f  <path-to-home-directory>\.ssh\<key-name> -C "<add-a-comment-to-the-key>"
> *Note* for the path to home directory, you can use "~" on macOS and Linux. It for some reason does not work on windows.

The command above does the following:

- ssh-keygen: command for creating key.
- -t xxxx: type of encryption to be applied.
- -f path: file path to where the key will be stored.
- -C "xxxx": comment is added to the key for personal reference.

The command would successfully create a key pair in your .ssh file. The private key will be called "key-name" while the public key will be called "key-name.pub".

---

## `doctl` on Linux

> *Note* this step can only be done on the Arch image since it is meant for linux. (why not for windows? My assignment didn't ask me to :])
`doctl` is the command line interface of DigitalOcean. It allows the user to directly use DigitalOcean from the terminal.

### Installing `doctl`

The following steps prepare the Linux system and downloads the CLI.

1. `sudo pacman -Syu`: This will update the system.
2. `sudo pacman -S doctl`: This will install doctl.
3. `Proceed with installation? [Y/n] y`: It will ask for confirmation to download. Press "y" on your keyboard.
4. `doctl version`: This will confirm if it got installed properly.

### Configuring `doct`

After installing `doctl`, you have to connect it to your digitalocean account to begin working. The configuration steps are as follows:

1. Create a digitalocean API token. These tokens work as a simple authentication key to connect your account to `doctl`.
   1. Go to the [Applications & API page](https://cloud.digitalocean.com/account/api/tokens).
   2. Click *Generate New Token* to begin creating the token.
   3. Type a *Token Name* by which the token will be reffered to.
   4. Choose an *Experation* time from the drop-down, for the token.
   5. Choose the *Scopes* you want the token owner to have.
   6. Click *Generate Token* to obtain it.
   > **Caution** After the token is generated, the token will be shown. This will only be shown the first time, so copy it somewhere safe.
   > ![Generating token panel](/attachments/image1.png)

   Now use the token in `doctl` to gain access to your account through the terminal. This will be done as follows:

2. `doctl auth init --context <name>`: This will begin the creation of the connection and name it.
3. `Enter your access token: <token>`: It will prompt you to enter the token. Put the earlier obtain token from step 1. here.
4. `doctl auth list`: The context option in step 2., neatly arranges multiple authenticated accounts in a list.
5. `doctl auth switch --context <name>`: This will allow you to switchto the desired account. (Do this step even if there is only one account.)
6. `doctl account get`: This will show your account information. Use this to confirm if you have successfully configured doctl.

With this you will be able to use your digitalocean account straight from the terminal.

---

## Add a Custom Arch Linux Image to DigitalOcean

Linux has many flavors (or dirtos or distributions), and Arch Linux is a popular one. It is a lightweight and flexible flavor meant to "Keep it Simple".

DigitalOcean is a cloud service provider. It aims to make managing your cloud easier, in its many use cases such as Cheap web hosting, Cloud VPN, Data streaming, Game development, Linux hosting, Image hosting, etc.

To use Arch Linux with digitalocean, obtain (or create) a Arch Linux image that is compatible with digitalocean, because digitalocean does not provide any Arch images by default. This image in techinical terms is called a disk image. They are compressed copy of data from a strorage device. DigitalOcean supports images in compressed format such as `.img`, `.qcow2`, or `.vmdk`.

> For this assignment i have used the custom image told to be used in the week 1 class. I used this [one](https://gitlab.archlinux.org/archlinux/arch-boxes/-/package_files/7529/download).

### Custom image on DigitalOcean

#### Image upload using web console

1. Open your browser and log in to your [DigitalOcean account](https://cloud.digitalocean.com).

2. From your [projects page](https://cloud.digitalocean.com/projects) follow the following steps:
   1. Click the drop-down *Manage*, in the left sidebar.
   2. Click *Backups & Snapshots*, under the drop-down menu.
   3. Click *Custom Images* section.
   >![Reaching custom image upload](/attachments/image2.png)

3. Upload Your Image:
   1. Click *Upload Image* and choose the image you had prepared in step 1.
   2. Type and edit the image name in *EDIT IMAGE NAME* .
   3. Choose the *Arch Linux* distribution.
   4. Select the *San Francisco* datacenter 3 as your region.
   5. Type some *Tags* and *Notes* for your image.
   6. Click *Upload Image* to confirm the image upload on digitalocean.
   >![Upload image panel](/attachments/image3.png)

4. DigitalOcean will take a few moments to process the image, and once completed it will be listed in the *Custom Images* section.

#### image upload using `doctl`

1. `doctl compute image create <name> --image-url <image-location> --region <name> --image-distribution Arch`: This creates a custom image in digitalocean.
2. `doctl compute image list`: This will show you the list of images you have. You can retrive your image ID from here, whenever you want.

> ![image creation in Linux](/attachments/image4.png)

Your digitalocean will reflect this image in a short while, indicating the tasks completion.

---

## DigitalOcean droplets

### Creating a droplet

#### Droplets on web console

#### Droplets on `doctl`

---

## Cloud-init

### Cloud-init configuration file

#### Using web console

#### Using `doctl`

---

## Connecting SSH keys

### SSH to server

#### On web console

#### On `doctl`

---

## References

[SSH defination](https://www.ssh.com/academy/ssh/protocol)
[SSH usage](https://www.cloudflare.com/learning/access-management/what-is-ssh/)
[What is Arch](https://archlinux.org/)
[Use of DigitalOcean](https://www.digitalocean.com/solutions/use-cases)
[What is doctl](https://docs.digitalocean.com/reference/doctl/)
[Custom image location](https://gitlab.archlinux.org/archlinux/arch-boxes/-/packages/1545)
[How to install and configure doctl](https://docs.digitalocean.com/reference/doctl/how-to/install/)
[What is a disk image](https://www.merriam-webster.com/dictionary/diskimage)
[Commands for doctl](https://docs.digitalocean.com/reference/doctl/reference/)
