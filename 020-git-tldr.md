<div align="center">
  <a href="https://git-scm.com/images/logo@2x.png">
    <img src="./img/git48x20.png"> 
    </br>
  </a>
</div>


# TL;DR ![img](./img/magit48x48.png)


## **Installation**

`guix install git`

`M-x magit`


## **Using Magit [ $ git init ] [ $ git status ]**

    magit-status (or "C-x g")

If file in current buffer not in a git repo, Emacs asks:

```
Create repository in ~/Nextcloud/computer/git/? 
```

Type

    "y"


## Magit [ $ git status ]

Run

    "C-x g"

again:

```shell
In the beginning there was darkness

Untracked files (6)
.#git.org
README.md
git.org
keepuntracked.org
readme.org
test-git.org
```

==> now there's a new directory in ~/Nextcloud/computer/git/ called \`.git', aka *local repository* or *working area*.

Now files are untracked. It turns out:

> You can see that your new README file is untracked, because it's under the "Untracked files" heading in your status output. Untracked basically means that Git sees a file you didn't have in the previous snapshot (commit), and which hasn't yet been staged; Git won't start including it in your commit snapshots until you explicitly tell it to do so.


## Magit [ $ git add] [ $ git add . ]

Type

    "s"

to track a file, you need to *stage* the file:

Emacs says:

```shell
Running git commit --
unable to auto-detect email address (got 'fhb@ThinkPad.(none)') ... [Hit $ to see buffer magit-process: git for details]
```


## **Configure git**

Ok, Git needs configuring &#x2026; there's no ~/.gitconfig yet&#x2026;

    $ git config --global user.name "Your Name"
    $ git config --global user.email "fhbarwegen@gmail.com"

==> now there is a .gitconfig, and can we commit now?


## \*Using Magit [git commit] \*

Yes (after restart) with:

    "C-x g" 

then see (something like):

```shell
Head:     master second commit

Untracked files (4)
README.md
keepuntracked.org
readme.org
test-git.org

Recent commits
ae24bb4 master second commit
1303281 first commit
```

If you now change a file (check that wiht git status, `"C-x g"`), you have to re-add that to the staging area!

    "c"  

to commit.

There's now have a local repository with a single commit. \*Let’s create a matching remote repository next.


## **The not so fun part&#x2026; creating the remote (on Github)**

Connecting the local repository with a single commit to a remote: **creating a matching remote repository, can not do do this in Magit yet** <sup><a id="fnr.1" class="footref" href="#fn.1" role="doc-backlink">1</a></sup>.

To connect repo to github via:

a. https with token (in stead of password); b. git via SSH with an SSH key.


## a. https with token (in stead of pasword)

Connect repo to Github: with token (in stead of password)

Now, there's an issue:

> remote: Support for password authentication was removed on August 13, 2021. remote: Please see <https://docs.github.com/en/get-started/getting-started-with-git/about-remote-repositories#cloning-with-https-urls> for information on currently recommended modes of authentication. fatal: Authentication failed for 'https://github.com/barwegen/git.git/'

Solution via [git - Support for password authentication was removed on August 13, 2021 - St&#x2026;](https://stackoverflow.com/questions/68781928/support-for-password-authentication-was-removed-on-august-13-2021) :

In order to fix the issue follow the below steps:

1.  Goto settings of Github account
2.  Find and Select Developer Settings
3.  Find and Select Personal access tokens
4.  Generate a new token
5.  Fill in any note and select the access scopes
6.  once done click on generate token
7.  Use the generated token in place of a password to communicate with GitHub.

You can NOT use the token via magit, so: $ git push -u origin master give your username, and in stead of the password this token.

-   ghp\_2aEyGkKa02f7MLdzh9wMNvpxjOx8Y7288azc **revoked**, use SSH

We do not want to use this token with every push, so:


## b. SSH - **Magit: better use SSH to connect with Github**

To get things working via Magit, [Magit: use password-store as auth source for Push and Pull operations?](https://www.reddit.com/r/emacs/comments/x0nf71/comment/imatskh/) : "Just Google for „git ssh how to use”; after you get it configured on your system, it will not require any further changes in Emacs or Magit, everything will just-work :-)" \*Turned out not really&#x2026; see sub 13. Magit keeps asking for the passprhase of the ssh key&#x2026;

==> Here is a good explanation: [Git SSH Keys: A Complete Tutorial | Atlassian Git Tutorial](https://www.atlassian.com/git/tutorials/git-ssh) ; then, this site: [About remote repositories - GitHub Docs](https://docs.github.com/en/get-started/getting-started-with-git/about-remote-repositories#cloning-with-https-urls) explains the various methods.


## **Generate an SSH key on Linux**

This is the tl;dr , more here: <../protocols/ssh.md> (sticks).

    $ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

You will then be prompted to "Enter a file in which to save the key." You can specify a file location or press “Enter” to accept the default file location.

    > Enter a file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter]

    > Enter passphrase (empty for no passphrase): [Type a passphrase]

    > Enter same passphrase again: [Type passphrase again]

Before adding the new SSH key to the ssh-agent first ensure the ssh-agent is running by executing:

    $ eval "$(ssh-agent -s)"

    > Agent pid 59566

Before using Git, add your key to ssh-agent: start ssh-agent if not started:

    $ eval `ssh-agent -s`

Once the ssh-agent is running the following command will add the new SSH key to the local SSH agent.

    $ ssh-add -K /Users/you/.ssh/id_rsa=

Now it works!


## **Now creating the remote repository**

The remote repository, can be created with:

-   [git push]
    -   create a repo on github
    -   set a remote &#x2013;> so git knows what github account to link to
    -   add your github credentials
    -   push the files to the remote repo
-   [git pull]
-   [git clone]


## **Create it with Git**

We allready have a local repo, so: Then [Managing remote repositories - GitHub Docs](https://docs.github.com/en/get-started/getting-started-with-git/managing-remote-repositories):

    $ git remote add origin git@github.com:barwegen/git.git

git remote add origin git@github.com:barwegen/testing.git

    $ git branch -M main

    $ git push -u origin main   ==>

    > ?Enter passphrase for key '/home/fhb/.ssh/id_rsa': 


## Aside: what is 'origin'

> Origin is just a default naming convention for referring to a remote Git repository. The point is that it is NOT github-specific. If it were, all generic git documentation that tells users how to do things that rely on the existence of a default name for this (ie: git push origin master) would become more complicated, as it would need to tell users how to figure out what the remote is named by their repo hosting provider, then how to do the actual command. &#x2013; source: [Why Remote for Github is named "origin" instead of "github" - Stack Overflow](https://stackoverflow.com/questions/9252272/why-remote-for-github-is-named-origin-instead-of-github)
> 
> Probably because you also get origin as remote name when you just git clone a repository.


## **Magit to push**

    "C-x g h P p" (‘magit-push-current-to-pushremote’)

This command pushes the current branch to its push-remote.

With a prefix argument or when the push-remote is either not configured or unusable, then let the user first configure the push-remote.

    > Enter passphrase for key '/home/fhb/.ssh/id.rsa':     


## **Magit asks for passphrase for ssh key every time**

Turns out, Magit (git) keeps asking, did this: [git - how to avoid being asked "Enter passphrase for key " when I'm doing ssh&#x2026;](https://superuser.com/questions/988185/how-to-avoid-being-asked-enter-passphrase-for-key-when-im-doing-ssh-operatio)

Still, magit (git) keeps asking, *Tarsius* is giving some info here: [Magit asks for passphrase for ssh key every time - Emacs Stack Exchange](https://emacs.stackexchange.com/questions/41343/magit-asks-for-passphrase-for-ssh-key-every-time) : (and for git in general here: [How to make git not prompt for passphrase for ssh key? - Super User](https://superuser.com/questions/1010542/how-to-make-git-not-prompt-for-passphrase-for-ssh-key).)

    $ guix install keychain

Add you private key to keychain:

    $ keychain --quiet id_rsa

prompted for:

=> Enter passphrase for key '*home/fhb*.ssh/id.rsa': =

    M-x install keychain-environment

```emacs-lisp
(use-package keychain-environment
  :config
  (keychain-refresh-environment))
```

and run:

```emacs-lisp
(keychain-refresh-environment)
```

Now Magit stops asking for the password for the ssh key, *all the time*, only a first time.

## Footnotes

<sup><a id="fn.1" class="footnum" href="#fnr.1">1</a></sup> <https://csm.hu/notes/2022/08/25/following-the-github-flow-with-emacs-and-magit/>