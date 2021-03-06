---
title: Beautiful Terminal Windows 
date: 2021-01-26T19:34:30-04:00
categories:
  - Blog
author: George Farcas
tags: [terminal, linux, zsh]
toc: true
toc_label: "Content"
header:
  image: "/assets/images/terminal.png"
  og_image: "/assets/images/terminal.png"
---
Who said terminals need to be monochrome? Follow this guide for a fancy colorful terminal and some modern alternatives to common utilities
## Start with the terminal
In order to have the above terminal prompt, we need to install the `zsh` shell and then install the [Powerlevel9k](https://github.com/Powerlevel9k/powerlevel9k) 

Install `zsh`:
```shell
sudo yum install zsh
```
Install the Powerlevel9k theme:
```shell
git clone https://github.com/bhilburn/powerlevel9k.git ~/powerlevel9k
echo 'source  ~/powerlevel9k/powerlevel9k.zsh-theme' >> ~/.zshrc
```
Run zsh shell by typing `zsh` and enter and voila! You have a fancy new terminal!

## The modern alternative of ls is exa
{:refdef: style="text-align: center;"}
![exa](https://github.com/ogham/exa/raw/master/screenshots.png)
{: refdef}
You cand find the instalation instructions at [exa GitHub page](https://github.com/ogham/exa
)

## A cat clone with wings, bat
{:refdef: style="text-align: center;"}
![bat](https://camo.githubusercontent.com/7b7c397acc5b91b4c4cf7756015185fe3c5f700f70d256a212de51294a0cf673/68747470733a2f2f696d6775722e636f6d2f724773646e44652e706e67)
{: refdef}
You cand find the instalation instructions at [bat GitHub page](https://github.com/sharkdp/bat)