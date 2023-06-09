# Let's create a dotfiles folder! ![img](./img/folder48x48.png)

-   Create the folder in the right location
-   Move some configuration files into it
-   Link them back to the original location
-   Verify that it works!


# Create a folder to store your dotfiles

I recommend creating this directory in the root of your home folder so that it's easier to use tools like GNU Stow:

```sh

mkdir ~/.dotfiles

```

Now we have a fresh new folder ready to be populated with files!


# Move some of your existing configuration files and folders into it

Move the configuration files you care about to an equivalent file path in `~/.dotfiles`:

```sh

mv ~/.emacs.d ~/.dotfiles/
mv ~/.bash_profile ~/.dotfiles

# ... etc ...

```

You should mirror the directory structure that the files have in your home folder on Linux and macOS so that dotfiles management tools can easily place the files where they belong.

On Windows this doesn't matter quite as much.


# Create symbolic links to the original config file locations

You can use the `ln` command on Linux and macOS to create symbolic links from a source file or directory to a new location:

```sh

# Create a new link called ~/.emacs.d which comes from ~/.dotfiles/.emacs.d
ln -sf ~/.dotfiles/.emacs.d ~/.emacs.d

```

We'll use this to create links back into the home directory for all the configuration files and folders we moved.


## For Windows users

On Windows, you can create a junction using `mklink`. To create a link for an **individual file**, use `mklink /H`:

```sh

mklink /H link-name.conf original-file.conf

```

To create a link for a directory, use `mklink /J`:

```sh

mklink /J c:\Users\david\AppData\Roaming\.emacs.d c:\Users\david\AppData\Roaming\.dotfiles\.emacs.d

```

**NOTE:** this command only works when you have started the Command Prompt (cmd.exe) as an administrator! Make sure to right click the icon and select "Run as Administrator" to launch an elevated prompt.


# The downside of symbolic links

As you might imagine, it's tedious to create and manage symbolic links like this, especially when you are syncing them between machines.

Some people solve this by writing a "bootstrapping" script that can create all symbolic links automatically.

It's easier to use a tool meant for this purpose, we will talk about GNU Stow and others!


# What's next?

Now that we have a dotfiles folder, the next step is to start managing it with Git!