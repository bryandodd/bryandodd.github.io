---
title: Bash Scripts
icon: material/bash
---

# Bash / ZSH Profile

## Formatting
``` zsh title=".zsh_formats"
color_nocolor='\033[0m'
color_yellow='\033[0;93m'
color_yellow2='\033[1;93m'
color_yellow_italic='\033[3;93m'
color_yellow_underline='\033[4;93m'
color_yellow_blink='\033[5;93m'
color_grey='\033[0;37m'
color_red='\033[0;31m'
color_red2='\033[1;31m'
color_green='\033[0;32m'
color_green2='\033[1;32m'
color_blue='\033[0;34m'
color_blue2='\033[1;34m'
color_purple='\033[0;35m'
color_purple2='\033[1;35m'
color_cyan='\033[0;36m'
color_cyan2='\033[1;36m'

greenplus='\033[1;32m[++]\033[0m'
greenarrow='\033[1;32m[->]\033[0m'
blinkgreenarrow='\033[1;32m[\033[5;32m->\033[0m\033[1;32m]\033[0m'
greenminus='\033[1;32m[--]\033[0m'
greenstar='\033[1;32m[**]\033[0m'
yellowstar='\033[1;93m[**]\033[0m'
yellowarrow='\033[1;93m[->]\033[0m'
blinkyellowarrow='\033[1;93m[\033[5;93m->\033[0m\033[1;93m]\033[0m'
bluestar='\033[1;34m[**]\033[0m'
cyanstar='\033[1;36m[**]\033[0m'
cyanarrow='\033[1;36m[->]\033[0m'
redminus='\033[1;31m[--]\033[0m'
redexclaim='\033[1;31m[!!]\033[0m'
redstar='\033[1;31m[**]\033[0m'
redarrow='\033[1;31m[->]\033[0m'
blinkredarrow='\033[1;31m[\033[5;31m->\033[0m\033[1;31m]\033[0m'
blinkwarn='\033[1;93m[\033[5;93m**\033[0m\033[1;93m]\033[0m'
blinkexclaim='\033[1;31m[\033[5;31m!!\033[0m\033[1;31m]\033[0m'
fourblinkexclaim='\033[1;31m[\033[5;31m!!!!\033[0m\033[1;31m]\033[0m'
```

## Aliases and Helpers

``` zsh title=".zsh_aliases"
# General
alias ll="eza -lahHgoO --icons=always --group-directories-first --time-style long-iso --git --git-repos"

alias up="cd .."
alias up2="cd ../.."
alias up3="cd ../../.."
alias up4="cd ../../../.."
alias up5="cd ../../../../.."

alias cat="bat -P"
alias cat-page="bat"

getinfo() {
    if [ -f $1 ]; then
        printf "\n${color_yellow2}File Information:${color_nocolor} \"stat -x ${1}\"\n"
        stat -x $1
        printf "\n${color_yellow2}Readlink:${color_nocolor} "
        readlink $1
        printf "\n${color_yellow2}Available Metadata:${color_nocolor}\n"
        exiftool $1
    else
        echo "${color_yellow_italic}File not found.${color_nocolor}"
    fi
}

# Network
alias listen='lsof -nP -i4 | grep LISTEN'
alias hosts='code /etc/hosts'
alias flushdns="sudo killall -HUP mDNSResponder;sudo killall mDNSResponderHelper;sudo dscacheutil -flushcache"

# Shell / macOS
alias profile='code ~/.zshrc'
alias sourceprofile='source ~/.zshrc'
alias fcount="find . -type f | wc -l"
alias newguid='uuidgen | tr "[:upper:]" "[:lower:]"'
alias listsnapshots="sudo tmutil listlocalsnapshots /"
alias delsnapshot="sudo tmutil deletelocalsnapshots "

# Pass / GoPass
alias pf="pass find "
alias gpf="gopass find "
alias gpl="gopass list "
alias gp="gopass "
alias gpo="gopass otp -o "
alias gpolist="gopass list mfa"
alias cpass="gopass --clip "

# Kubernetes
alias getKubeSA='kubectl get sa -o custom-columns="SERVICE-ACCOUNTS":.metadata.name,"NAMESPACE":.metadata.namespace,"ROLE":.metadata.annotations."eks\.amazonaws\.com/role-arn","CREATED":.metadata.creationTimestamp'
alias getKubeService='kubectl get services -o wide'
alias getKubePods="customPodInfo "

customPodInfo() {
    cContext=$(kubectl config current-context)
    cNamespace=$(kubens -c)
    echo "Context: ${color_cyan2}${cContext}${color_nocolor}"
    echo "Namespace: ${color_cyan2}${cNamespace}${color_nocolor}"
    echo -e "\n${color_yellow2}=== Standard Pod Information ===${color_nocolor}\n"
    kubectl get pods -o wide 
    echo -e "\n${color_yellow2}=== Extended Pod Information ===${color_nocolor}\n"
    kubectl get pods -o custom-columns="POD-NAME":.metadata.name,"APP":.metadata.labels.app,"CONTAINERS":'.spec.containers[*].name',"SVC-ACCT":.spec.serviceAccount,"MEM-REQ":'.spec.containers[*].resources.requests.memory',"MEM-MAX":'.spec.containers[*].resources.limits.memory',"CPU-REQ":'.spec.containers[*].resources.requests.cpu',"CPU-MAX":'.spec.containers[*].resources.limits.cpu'
}

# Web / Testing
alias decode="base64decode "
base64decode() { echo "$1" | base64 --decode; echo; }

alias encode="base64encode "
base64encode() { echo -n "$1" | base64; echo; }

alias encodefile="base64file "
base64file() { openssl base64 -A -in $1 -out $2; }

expandUrl() { curl -sIL $1 | grep ^Location; }
webtest() { curl -Is $1 -L | grep HTTP/; }
webheaders() { curl -I --http2 $1 }

# Miscellaneous
alias fingerprint="ssh-keygen -lf "

fdiff() {
    if [[ "$#" == "2" ]]; then
        diff -u $1 $2 | diff-so-fancy
    else
        printf "\n ${blinkwarn} Only two inputs supported."
    fi
}

fdiffside() {
    if [[ "$#" == "2" ]]; then
        delta --paging never --dark --side-by-side --line-numbers $1 $2
    else
        printf "\n ${blinkwarn} Only two inputs supported."
    fi
}
```