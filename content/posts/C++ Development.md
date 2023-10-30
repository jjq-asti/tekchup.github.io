+++
title = "C/C++ Development in Emacs"
author = ["jeanjayquitayen"]
date = 2023-10-30T00:00:00+08:00
categories = ["Tutorials"]
draft = false
+++

## Introduction {#introduction}

I do most of my software projects in Doom Emacs. I use python most of the time until recently when I decided to learn C and C++ again. I do have experience in both of them most specially C since I do embedded projects back at school and in some work related projects before.

The features that I am looking for are code completion, syntax checking and linting.
To get this features I need to active `lsp` and install the LSP server on the host machine. The C/C++ language mode should also be activated in Doom Emacs.


## Install LSP server {#install-lsp-server}

I use **ccls** for the language server because it is the most suggested server and it works great in my experience.

I installed it using my system's package manager.
`sudo zypper install ccls`
This should also be available on other distros (i.e. Ubuntu).


## Setup emacs {#setup-emacs}

1.  Activate lsp and C/C++ mode in the `init.el` file at the `:tools` section.
    ```emacs-lisp
          (lsp)
    ```
    and activate C/C++ languange in `:languages` section
    ```emacs-lisp
          (cc +lsp)
    ```

2.  Reload doom emacs by pressing `SPC-h r r`.
    I am running emacs in client mode so I also restart emacs to be sure.

    ```shell
          systemctl --user restart emacs
    ```
