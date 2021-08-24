---
title: Bash codes for custom commands
weight: 1
menu:
  codes:
    name: For custom commands
    identifier: codes-bashScripts-customCodes
    parent: codes-bashScripts
    weight: 1
---


{{< note title="Bash codes for custom commands" >}}

```bash

alias goto='/mnt/c/Program\ Files/Opera/launcher.exe'
#alias search='goto www.google.com/search?q='
g() {
    search=""
    echo "Googling: $@"
    for term in $@; do
        search="$search%20$term"
    done
        search=${search/\%20/}
    goto http://www.google.com/search?q=$search
        }
y(){
        sea=""
        echo "Searching in Youtube: $@"
        for term in $@; do
        sea="$sea+$term"
        done
        sea=${sea/\+/}
        goto https://www.youtube.com/results?search_query=$sea
        }
alias km='/mnt/c/Program\ Files/KMPlayer/KMPlayer.exe'
op(){
        nil=""
        dmn=""
        sl="/"
        file="file:///"
        col=":/"
        loc=$(pwd)
        link=${loc/\/mnt\//$nil}
        base=$(removebackslash $@)
        for term in $link$sl$base; do
           dmn="$dmn%20$term"
        done

        dmn=${dmn/\%20/$nil}
        dmn=${dmn/\//$col}
        dmn=$file$dmn
        echo "$dmn"
        echo "Opening $@ in Opera"
        goto $dmn
    }
removebackslash(){

        bs="\\"
        str=${@//$bs/}
        echo "$str"
        }
```
{{< /note >}}
