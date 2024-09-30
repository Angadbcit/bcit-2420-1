# This is my assignment 1

All levels (1-3) are done with refernces provided at the bottom and in-text.

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

SSH stands for Secure Shell. It is a protocol used to send commands to a computer over an unsecured network in a secure manner. The connection works in a client-server model. The method of securing the connection is done through the method of cryptography. [1](https://www.cloudflare.com/learning/access-management/what-is-ssh/)  It is a 2 step process:

1. Authentication: Verify the identity of the SSH server.
2. Encryption: A strong symetric encryption algorithm is used, which is transferred over the network using the standard hash algorithms.

When we make a SSH key, we are in reality making a pair of keys. They are the public and private SSH keys. They serve a simple purpose of authenticating the client. The cryptographic pair of keys are created on the client's device, and once created, the public key is shared with the server. Now the keys will act as passwords, so when the client wants to log in, the server will compare the public key to the private key on the client's end. If they match, the connection will be established. [2](https://www.ssh.com/academy/ssh/protocol)

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

`doctl` is the command line interface of DigitalOcean. It allows the user to directly use DigitalOcean from the terminal. [3](https://docs.digitalocean.com/reference/doctl/)

### Installing `doctl`

The following steps prepare the Linux system and downloads the CLI. [4](https://docs.digitalocean.com/reference/doctl/how-to/install/)

1. `sudo pacman -Syu`: This will update the system.
2. `sudo pacman -S doctl`: This will install doctl.
3. `Proceed with installation? [Y/n] y`: It will ask for confirmation to download. Press "y" on your keyboard.
4. `doctl version`: This will confirm if it got installed properly.

### Configuring `doct`

After installing `doctl`, you have to connect it to your digitalocean account to begin working. The configuration steps are as follows: [4](https://docs.digitalocean.com/reference/doctl/how-to/install/)

1. Create a digitalocean API token. These tokens work as a simple authentication key to connect your account to `doctl`.
   1. Go to the [Applications & API page](https://cloud.digitalocean.com/account/api/tokens).
   2. Click *Generate New Token* to begin creating the token.
   3. Type a *Token Name* by which the token will be reffered to.
   4. Choose an *Experation* time from the drop-down, for the token.
   5. Choose the *Scopes* you want the token owner to have.
   6. Click *Generate Token* to obtain it.
   > **Caution** After the token is generated, the token will be shown. This will only be shown the first time, so copy it somewhere safe.
   > ![Generating token panel](/attachments/image1.1.png)  [5](https://cloud.digitalocean.com/account/api/tokens/new)

   Now use the token in `doctl` to gain access to your account through the terminal. This will be done as follows:

2. `doctl auth init --context <name>`: This will begin the creation of the connection and name it.
3. `Enter your access token: <token>`: It will prompt you to enter the token. Put the earlier obtain token from step 1. here.
4. `doctl auth list`: The context option in step 2., neatly arranges multiple authenticated accounts in a list.
5. `doctl auth switch --context <name>`: This will allow you to switchto the desired account. (Do this step even if there is only one account.)
6. `doctl account get`: This will show your account information. Use this to confirm if you have successfully configured doctl.

With this you will be able to use your digitalocean account straight from the terminal.

---

## Add a Custom Arch Linux Image to DigitalOcean

Linux has many flavors (or dirtos or distributions), and Arch Linux is a popular one. It is a lightweight and flexible flavor meant to "Keep it Simple". [6](https://archlinux.org/)

DigitalOcean is a cloud service provider. It aims to make managing your cloud easier, in its many use cases such as Cheap web hosting, Cloud VPN, Data streaming, Game development, Linux hosting, Image hosting, etc. [7](https://www.digitalocean.com/solutions/use-cases)

To use Arch Linux with digitalocean, obtain (or create) a Arch Linux image that is compatible with digitalocean, because digitalocean does not provide any Arch images by default. This image in techinical terms is called a disk image. They are compressed copy of data from a strorage device. [8](https://www.merriam-webster.com/dictionary/diskimage)  DigitalOcean supports images in compressed format such as `.img`, `.qcow2`, or `.vmdk`.

> For this assignment i have used the custom image told to be used in the week 1 class. [9](https://gitlab.archlinux.org/archlinux/arch-boxes/-/package_files/7529/download)

### Custom image on DigitalOcean

#### Image upload using web console

1. Open your browser and log in to your [DigitalOcean account](https://cloud.digitalocean.com).

2. From your [projects page](https://cloud.digitalocean.com/projects) follow the following steps:
   1. Click the drop-down *Manage*, in the left sidebar.
   2. Click *Backups & Snapshots*, under the drop-down menu.
   3. Click *Custom Images* section.
   >![Reaching custom image upload](/attachments/image1.2.png)  [10](https://cloud.digitalocean.com/images/snapshots/custom_images)

3. Upload Your Image:
   1. Click *Upload Image* and choose the image you had prepared in step 1.
   2. Type and edit the image name in *EDIT IMAGE NAME* .
   3. Choose the *Arch Linux* distribution.
   4. Select the *San Francisco* datacenter 3 as your region.
   5. Type some *Tags* and *Notes* for your image.
   6. Click *Upload Image* to confirm the image upload on digitalocean.
   >![Upload image panel](/attachments/image1.3.png)  [10](https://cloud.digitalocean.com/images/snapshots/custom_images)

4. DigitalOcean will take a few moments to process the image, and once completed it will be listed in the *Custom Images* section.

#### image upload using `doctl`

[Commands from here.](https://docs.digitalocean.com/reference/doctl/reference/compute/image/)

1. `doctl compute image create <name> --image-url <image-location> --region <name> --image-distribution Arch`: This creates a custom image in digitalocean.
2. `doctl compute image list`: This will show you the list of images you have. You can retrive your image ID from here, whenever you want.

> ![image creation in Linux](/attachments/image1.4.png)

Your digitalocean will reflect this image in a short while, indicating the tasks completion.

---

## DigitalOcean droplets

DigitalOcean allows for easy creation of cloud based virtual machines, which are called droplets. Droplets are meant to be an easy to set up virtual machine on the cloud at an affordable price. Droplets also boost high and reliable scalability and secure connections. It allows for the creation of a wide variety of machines based on your needs.

### Creating a droplet

Creating a droplet is a job of few clicks or one line (doctl) away. The steps are provided below.

#### Droplets on web console

1. From your [projects page](https://cloud.digitalocean.com/projects) follow the following steps:
   1. Click the drop-down *Create* from the topbar in the green color.
   2. Choose *Droplets* to begin creating a cloud server.
2. Choose an appropriate *Region* for your area.
3. Choose a *Datacenter* from the drop-down. Select the one that is closest to you geographically to avoid latency.
4. Choose *Custom images* under the *Choose an image* section.
5. Choose the custom image you previously added to digitalocean.
6. Select an appropriate size for your virtual machine based on your needs:
   1. Choose a *Droplet Type*.
   2. Choose the *CPU options* based on your droplet type and needs.
7. Click *SSH Key* under Choose Authentication Method.
8. Choose at least one of the added SSH keys or create a new key.
9. Scroll down to *Finalize Details* to finish up the proccess:
   1. Choose the number of droplets you want to create.
   2. Type a name for the droplet.
   3. Give it any tags for better organization of droplets.
   4. Select in which project the droplets sshould be saved in.
10. Click *Create Droplet* to finish creating a working VM.

> ![Droplet creation step 1-5](/attachments/image1.5.1.png)
> ![Droplet creation step 6](/attachments/image1.5.1.png)
> ![Droplet creation step 7-10](/attachments/image1.5.1.png)
> [11](https://cloud.digitalocean.com/droplets/new)
>
> *Note* that there were a few other options to select while creating the droplet as well. They are all need based choices, and are not necessary. If you want to apply them, feel free to do so.

After the successful creation of the droplet, it will appear under your *Resources* section of the project you selected it to be saved in.

#### Droplets on `doctl`

[Commands from here.](https://docs.digitalocean.com/reference/doctl/reference/compute/droplet/)

1. `doctl compute droplet create <name> --size <x-xvcpu-xgb-xxgb-xxxx> --image <ID> --region <xxxx> --ssh-keys <ID> --tag-name <xxx>`: This is the syntax of the command to create a droplet. The <> and "x" need to be replaced with the appropriate values.
2. `doctl compute droplet list`: This can be used to confirm if the droplet was successfully created.

> *Note* that at the moment `doctl` is only capable of creating one droplet at a time.

---

## Cloud-init

Cloud-init is a utility that allows us to set-up some intial configurations for our machines to run automatically during system boot. It is a revolutionary creation as it has greaty decreased the burden while configuring a host name, installing packages on an instance, running scripts, suppressing default virtual machine (VM) behavior, etc.  [12](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_and_managing_cloud-init_for_rhel_9/introduction-to-cloud-init_cloud-content#introduction-to-cloud-init_cloud-content)

Most cloud services support cloud-init, with it coming pre-setup on many Linux flavors. These configurations are also not limited, as most roviders allow for multiple ways of configuring files.

### Cloud-init configuration file

Cloud-init uses yml/yaml file formatting types to run. The cloud config file i.e., the yaml follows a format similar to this:

    users: # creates a user
    name: <name>
    primary_group: <name>
    groups: wheel
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys: # adds the SSH key
        <public-SSH-key>
    
    packages: # installs metioned packages
        ripgrep
        rsync
        neovim
        fd
        less
        man-db
        bash-completion
        tmux
    
    disable_root: true

The [Cloud file](/cloud-config.yml) I used for the assignment is this.

#### Adding with web console

1. Do steps 1-8 from *DigitalOcean droplets < Creating a droplet < Droplets on web console*.
2. Click *Advanced Options* under Choose Authentication Method.
3. Click *Add Initialization scripts (free)* check box to add a cloudscript to run at initialization.
4. Type your script in the *Enter user data here...* text box.
5. Do steps 9 and 10 from *DigitalOcean droplets < Creating a droplet < Droplets on web console*.

> ![Script adding on web browser](/attachments/image1.6.png)  [11](https://cloud.digitalocean.com/droplets/new)

This droplet created will be under your *Resources* section of the project you saved in. Upon running the droplet for the first time, the script will automatically run, initializing your droplet.

#### Adding with `doctl`

[Commands from here.](https://docs.digitalocean.com/reference/doctl/reference/compute/droplet/create/)

1. `cd ~`: Go to your home directory.
2. `mkdir config`: Creates a directory named config.
3. `cd config`: Move to the newly created config directory.
4. `nvim <file-name>.yml`: Creates a yml file, add enter text editor.
5. Press "**i**" to enter insert mode to edit file.
6. Add your yml file content.
7. Press "**ESC**" to exit insert mode.
8. `:wq`: Type it to save and quit the yml file.
9. `doctl compute droplet create <name> --size <x-xvcpu-xgb-xxgb-xxxx> --image <xxx> --region <xxxx> --ssh-keys <xxx> --tag-name <xxx> --user-data-file <~/config/file-name.yml>`: This will create a droplet with the yml configurations you input.
10. `doctl compute droplet list`: This can be used to confirm if the droplet was successfully created.

![Droplet creation in linux with cloud-init](/attachments/image1.7.png)

---

## Connecting SSH keys to server

To use SSH keys in digitalocean, we must first connect the key to the cloud service provider.

### SHH key connecting

First copy the contents of the public SSH key to use while connecting the key to the server.

**Windows:** `Get-Content C:\Users\<your-user-name>\.ssh\<key-name>.pub | Set-Clipboard`: This copies the key onto your clipboard for later use.

#### On web console

1. Do steps 1-6 from *DigitalOcean droplets < Creating a droplet < Droplets on web console*.
2. Click *New SSH Key* to open the panel for creating adding a key.
3. Paste the earlier copied public key in *SSH key content*.
4. Type a name for the key.
5. Click *Add SSH Key*.
6. Select the newly created key.
7. Continue with the rest of the steps for the creation of a droplet.

> ![Add a new key panel](/attachments/image1.8.png)  [11](https://cloud.digitalocean.com/droplets/new)

Successful droplet creation indicates that the SSH key was correct.

#### On `doctl`

[Commands from here.](https://docs.digitalocean.com/reference/doctl/reference/compute/ssh-key/)

1. `doctl compute ssh-key import <key-name> --public-key-file ~/.ssh/<name>.pub`: This copies the public key from the file and imports it to digitalocean.
2. `doctl compute ssh-key list`: Confirm key creation and ID from here.

---

## References

[SSH defination](https://www.cloudflare.com/learning/access-management/what-is-ssh/)

[SSH description](https://www.ssh.com/academy/ssh/protocol)

[What is doctl](https://docs.digitalocean.com/reference/doctl/)

[How to install and configure doctl](https://docs.digitalocean.com/reference/doctl/how-to/install/)

[Token generation](https://cloud.digitalocean.com/account/api/tokens)

[What is Arch](https://archlinux.org/)

[Use of DigitalOcean](https://www.digitalocean.com/solutions/use-cases)

[What is a disk image](https://www.merriam-webster.com/dictionary/diskimage)

[Custom image](https://gitlab.archlinux.org/archlinux/arch-boxes/-/package_files/7529/download)

[DigitalOcean projects](https://cloud.digitalocean.com/projects)

[Custom image upload](https://cloud.digitalocean.com/images/snapshots/custom_images)

[Droplets](https://www.digitalocean.com/products/droplets)

[Creating droplets](https://cloud.digitalocean.com/droplets/new)

[What is cloud-init](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_and_managing_cloud-init_for_rhel_9/introduction-to-cloud-init_cloud-content#introduction-to-cloud-init_cloud-content)

[Commands for doctl](https://docs.digitalocean.com/reference/doctl/reference/)
