---
title: How to Set Up a C language environment on Manjaro
description: Just a short notes of what I had to install to get ready to compile some C language snippets in Manjaro.
categories:
 - tutorial
tags: c
---

# How to Set Up a C language environment on Manjaro

Just a short notes of what I had to install to get ready to compile some C language snippets in Manjaro.

I had previously installed Ubuntu, and there the package requiered for compiling C code was *build-essentials*, but I didn't know if there was a equivalent in Arch. A quick search on google point me to [*base-devel*](https://www.archlinux.org/groups/x86_64/base-devel/) package. So a simple:

```bash
sudo pacman -Sy base-devel
```
Got everything in place.

Time for a quick test:

```C
#include <stdio.h>

int main(void)
  {
    printf("Hello World\n");

    return 0;
  }
```

then:

```bash
make main
```

And thats all!
