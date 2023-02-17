---
layout: post
title: "My First Patchset"
date: 2023-02-15 11:57:26 -0300
category: kernel developing
tags: GSoC open-source
---

Hello everyone! In this Post, I will describe my experience writing my first patch set to the Linux
Kernel, more specifically in the [Direct Rendering Manager (DRM)][wikipedia-drm] subsystem.

## Why I'm Doing This

Two years ago, when I started university, I became excited about open-source software. Before
university, I had heard about Linux, Vim, and other open-source projects, but never used or read
much about them.

I've found that using those technologies made my workflow much better. When I entered an
extracurricular group called [Zenith][zenith], I met some friends there, like [Maíra
Canal][maira-canal], that were already interested in contributing.

My first contribution was a [patch][first-patch-co-developed] started in a hackathon, made by
[LKCAMP][lkcamp], to the Linux Kernel that I co-developed. The experience of creating it was quite
interesting. After it, I contributed a couple more times, [once to the LLVM][llvm-patch] and again
to the Linux Kernel, where I made [my first own patch][my-first-own-patch].

Now I feel that the [GSoC 2023][gsoc] will be a perfect opportunity to work even more in the
open-source community, something that I really like to do. This patchset is part of my initiation on
the [GSoC project Idea about Increasing Code Coverage on the DRM code][xorg-gsoc-project-idea]. The
main idea is to increase the test coverage of the DRM core helpers by adding more [KUnit][kunit]
tests to it.

## Patchset Idea

The idea is to decrease number of warnings generated when compiling the AMD GPU drivers with `W=1`.
This is a pretty simple task to start working on, as is not needed an specific hardware to test it.

Sometimes this work could be viewed as not useful, but it is a much-needed job that sometimes is put
aside on behalf of more urgent problems. It makes the code less error-prone, more readable, and
simpler, helping others in developing.

## Set Up

To start you can clone the [`drm-misc`][drm-misc] Linux Kernel tree as it was recommend. (If you
want, you can use the [`amd-staging-drm-next`][amd-staging-drm-next] branch too ;) ).

```bash
git clone git://anongit.freedesktop.org/drm/drm-misc
```

Or you can add the remote to an existing local repository of the Linux Kernel. (That was what I
did :P).

```bash
git remote add drm-misc git://anongit.freedesktop.org/drm/drm-misc
```

And create a local branch based on the `drm-misc-next` branch.

```bash
git checkout drm-misc/drm-misc-next
git checkout -b fix-warning
```

After that I used the defconfig and added the AMDGPU config as a module with the menuconfig.

```bash
make defconfig
make menuconfig # Add AMDGPU as a module
```

## Workflow

The idea is trying to resolve the warnings that appear when building the AMDGPU code. Considering
the kernel uses the `-Werror` flag, the build will stop after any warning, which can make the
development process a bit tricky. To speed up the process it is possible to add `-Wno-error` to
`KCFLAGS` in the make command with the `W=1`. This way you be able to resolve multiple warnings in
one go.

```bash
make W=1 KCFLAGS='-Wno-error' modules
```

This will compile the module and generate all the warnings, which can be a bit overwhelming as all
types of warnings appear together.


To focus into one warning you can remove the `W=1` flag and add the specific to `KCFLAGS`, so the
final command would be:

```bash
make KCFLAGS='-Wno-error -W<specific-warning>' modules
```

## What was done

The patch set focused in four types of warnings:

- `-Wunused-but-set-variables`

	Warn when a variable has its value changed but never used in any other way. The warning
	could be indicative that the code its not doing what it was meant to do. It also makes the code
	less readable with dead code.

- `-Wenum-conversion`

	Warn when a value of enumerated type is implicitly converted to a different enumerated type.

- `-Wmissing-prototypes`

	Warn if a global function is defined without a previous prototype declaration.

- Excess arguments on Kernel docs

	Warn when arguments are described on the kernel doc but not present on the function definition.

At first the set was composed only by 4 patches, one for each type of warning, but after some advice
from [Maíra Canal][maira-canal] they were split even more as different tactics were used to fix the
same type of problem in different places.

## How I Sent

For a more in depth procedure, please read the [kernel documentation about this
topic][kernel-doc-applying-patch].

First, I used the `check-patch` utility to check all the commits for any code style error

```bash
./scripts/check-patch --strict -g drm-misc/drm-misc-next
```

Arguments:
- `--strict`: enable more subjective tests
- `-g drm-misc/drm-misc-next`: check all the commit until the drm-misc-next branch

To format the patchset I used the git format-patch utility

```bash
# On the fix-warning branch
git format-patch drm-misc/drm-misc-next -o outgoing --cover-letter
```

Arguments:
- `drm-misc/drm-misc-next`: get all the commits until the drm-misc-next branch
- `-o outgoing`: place the patches on the outgoing directory
- `--cover-letter`: generate a first patch file with a cover letter to write about the whole patch

After I wrote my cover letter and made various sanity checks such as: compiling the kernel one more
time and reading the patch files to spot any errors. I used the `get_maintainer` utility to find the
maintainers and mailing lists to which I had to send my patches.

```bash
./scripts/get_maintainer.pl outgoing/*
```

Arguments:
- `outgoing/*`: read all the patches inside the `outgoing` directory

After that I used the `git send-email` to send the patches to all the emails that were maintainers
or supporters on the `get_maintainer` output and to the dri-devel and amd-gfx mailing lists.

```bash
git send-email \
--to='amd-gfx@lists.freedesktop.org, dri-devel@lists.freedesktop.org' \
--cc='<emails-of-maintainers-and-subscribers>' outgoing/*
```

To a tutorial on how to setup git send-email, I recommend the [sourcehut
tutorial][sourcehut-tutorial].

After all this work I needed to wait for responses and hope that no dumb errors were present :).

## How the Patch was Received

Fortunately the patches had a pretty good response!

The [first version of the patch set][first-patchset-version] had 10 patches and six of them were
accepted with no requests for changes. The other four received different responses: One was thought
unnecessary, another hasn't had any response yet, and the other two were requested some
modification.

Those last couple ones will be sent on the second version of this patch.

##  What To Do Next

After this first version I'll send a v2 with the 2 patches that were asked revision and the patch
that did not receive any response on the v1.

After that, I intend to keep preparing myself for [GSoC 2023][gsoc]! I'm thinking of exploring the
unit test on the DRM side with [Kunit][kunit]. But this will be explained better on a next blog
post.

See ya! :)

[wikipedia-drm]: https://en.wikipedia.org/wiki/Direct_Rendering_Manager
[zenith]: https://zenith.eesc.usp.br/
[maira-canal]: https://mairacanal.github.io/
[first-patch-co-developed]: https://lore.kernel.org/all/20220615135824.15522-1-maira.canal@usp.br/
[lkcamp]: https://lkcamp.dev/about/
[llvm-patch]: https://github.com/llvm/llvm-project/commit/1534b048d6fdd7cf5f7dd5d3b8c6876b7cdad184
[my-first-own-patch]:  https://lore.kernel.org/all/20221026211458.68432-1-arthurgrillo@riseup.net/
[gsoc]: https://summerofcode.withgoogle.com/
[xorg-gsoc-project-idea]: https://www.x.org/wiki/DRMcoverage2023/
[drm-misc]: https://cgit.freedesktop.org/drm/drm-misc/
[amd-staging-drm-next]: https://gitlab.freedesktop.org/agd5f/linux/-/tree/amd-staging-drm-next
[kernel-doc-applying-patch]: https://www.kernel.org/doc/html/latest/process/submitting-patches.html
[sourcehut-tutorial]: https://git-send-email.io/
[first-patchset-version]: https://lore.kernel.org/all/20230213204923.111948-1-arthurgrillo@riseup.net/
[kunit]: https://www.kernel.org/doc/html/latest/dev-tools/kunit/index.html
