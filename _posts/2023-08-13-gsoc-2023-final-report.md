---
layout: post
title: "GSoC 2023 Final Report"
date: 2023-08-27 18:10:00 -0300
category: kernel developing
tags: GSoC
---

The GSoC journey is coming to a close. In just over 100 days, I gained
more experience in open-source development than I could ever imagine
in this period.

Prior to GSoC, I was not used to regularly submit patches to the
mailing lists. Now, I've sent many patches and revisions. I believe my
interaction with the community will only grow. I learned so much about
the tools and workflow of kernel development.

After this experience, I'm more than certain that I want to make this a
job, contributing to open-source is fun, so why not make this a living
:)

# Goals

The main goal of the project was to increase the code coverage on the
DRM core helper functions by creating unit tests.

As the coverage of all helpers is a big task for the time period, I
decided to create tests for the `drm_format_helper.c` functions.

Throughout the project, other side tasks appeared. I will list the
contributions made below.

# GSoC contributions

## Linux Kernel - VKMS

VKMS is a software-only model of a KMS driver that is useful for
testing and running X (or similar) on headless machines.

This was, unexpectedly, a big part of my GSoC. I learned a lot about
color formats and how a graphics driver works. Currently, only one
piece of my work was upstreamed, the rest needs more work and was
postponed in favor of the primary project goal.

| Patch | Status |
|-------------------------------------------------------------------------------------------------------------------------|--------------|
| [drm/vkms: Add support to 1D gamma LUT](https://lore.kernel.org/all/20230621194121.184552-1-arthurgrillo@riseup.net/#r) | **Accepted** |

For more information go check my [blogpost]({{site.baseurl}}{%
link _posts/2023-06-19-nv12-vkms.md %}) about the whole process.

## IGT

IGT GPU Tools is a collection of tools for the development and testing
of DRM drivers. While working on VKMS I used heavily the IGT framework
for testing, in one occasion a bug made a test to stop working on the
VKMS, so a submitted a patch to fix that.

| Patch                                                                                                                           | Status       |
|---------------------------------------------------------------------------------------------------------------------------------|--------------|
| [lib/igt_fb: Add check for intel device on use_enginecopy](https://patchwork.freedesktop.org/patch/545338/?series=120144&rev=1) | **Accepted** |

## Linux Kernel - DRM

In the DRM subsystem, I've done the main project goal, contributed by
adding unit tests, and also helped to fix some bugs that appeared
while working on the tests. With the sent patches I got [71.5% of line
coverage and 85.7% of function
coverage](https://grillo-0.github.io/coverage-reports/gsoc-drm-format-test/drivers/gpu/drm/drm_format_helper.c.gcov.html)
on the `drm_format_helper.c`.

| Patch                                                                                                                                                   | Status       |
|---------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| [drm/tests: Remove CONFIG_DRM_FBDEV_EMULATION on .kunitconfig](https://lore.kernel.org/r/20230726220325.278976-1-arthurgrillo@riseup.net/)              | **Rejected** |
| [drm/tests: Alloc drm_device on drm_exec tests](https://lore.kernel.org/r/20230731182241.240556-1-arthurgrillo@riseup.net/)                             | **Accepted** |
| [drm/tests: Test default pitch fallback](https://lore.kernel.org/r/20230814-gsoc-drm-format-test-v2-v3-1-bd3e9f9bc2fb@riseup.net/)                      | **Accepted** |
| [drm/tests: Add KUnit tests for drm_fb_swab()](https://lore.kernel.org/r/20230814-gsoc-drm-format-test-v2-v3-2-bd3e9f9bc2fb@riseup.net/)                | **Accepted** |
| [drm/tests: Add KUnit tests for drm_fb_clip_offset()](https://lore.kernel.org/r/20230814-gsoc-drm-format-test-v2-v3-3-bd3e9f9bc2fb@riseup.net/)         | **Accepted** |
| [drm/tests: Add KUnit tests for drm_fb_build_fourcc_list()](https://lore.kernel.org/r/20230814-gsoc-drm-format-test-v2-v3-4-bd3e9f9bc2fb@riseup.net/)   | **Accepted** |
| [drm/tests: Add multi-plane support to conversion_buf_size()](https://lore.kernel.org/r/20230814-gsoc-drm-format-test-v2-v3-5-bd3e9f9bc2fb@riseup.net/) | **Accepted** |
| [drm/tests: Add KUnit tests for drm_fb_memcpy()](https://lore.kernel.org/r/20230814-gsoc-drm-format-test-v2-v3-6-bd3e9f9bc2fb@riseup.net/)              | **Accepted** |

# Challenges Faced

I think the most difficult task was describing my work. Either on blog
posts or in the commit messages, it takes a lot of work to write what
you've done concisely and clearly. With time you get the way of
things, but I think I can improve on this subject.

Moreover, many times I had to debug some problems. I already knew how
to use GDB, but using it in the kernel is a little more
cumbersome. After searching, reading the documentation, and getting
tips from my mentors, I got it working.

On the VKMS, I had to create new features, this requires a lot of
thought. I made a lot of diagrams in my head to understand how the
color formats would be displayed in memory, and yet most of my work
hasn't seen the light of day XD.

# What is left to do

I was able to do most of the proposed tasks. But the `drm_xfrm_toio`
was left out due to the difficulty of testing it, as it uses IO
memory. I tested the `drm_fb_blit()`, but I'm waiting for the
acceptance of the patchset to send it, with that patch the [line
coverage will go to 89.2% and the function coverage will go to
94.3%](https://grillo-0.github.io/coverage-reports/final-gsoc/drivers/gpu/drm/drm_format_helper.c.gcov.html).

# Community Interaction

Besides patch submission, I reviewed some patches too. Going to the
other side, I enjoyed thinking about how a piece of code
could be improved.

Also, on one occasion I started a discussion about the best way to
solve an issue by sending a
[patch](https://lore.kernel.org/r/20230726220325.278976-1-arthurgrillo@riseup.net/). This
got me a Reported-by tag on the [patch that fixed the
bug](https://lore.kernel.org/all/20230804125156.1387542-1-javierm@redhat.com/).

| Patch                                                                                                                                                                  |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Re: [PATCH] drm/format_helper: Add Kunit tests for drm_fb_xrgb8888_to_mono()](https://lore.kernel.org/r/782e6705-9799-b87e-60fd-ad88031ff909@riseup.net/)             |
| [Re: [PATCH] drm/format_helper: Add Kunit tests for drm_fb_xrgb8888_to_mono()](https://lore.kernel.org/r/6cddfeb2-ceb6-fcb6-07bd-df3317c05a64@riseup.net/)             |
| [Re: [PATCH v2] drm/format-helper: Make conversion_buf_size() support sub-byte pixel fmts](https://lore.kernel.org/r/6a3d0907-5e04-5e72-cf45-3eb619ea6efb@riseup.net/) |
| [Re: [PATCH v3 1/2] drm/format-helper: Add Kunit tests for drm_fb_xrgb8888_to_mono()](https://lore.kernel.org/r/23609fe6-3413-e034-6900-5ad3be082ca4@riseup.net/)      |
| [Re: [PATCH 1/5] drm/tests: Test drm_rect_intersect()](https://lore.kernel.org/r/64ca1b2a-8a21-2a3e-bea8-f40ae9383194@riseup.net/)                                     |
| [Re: [PATCH v2 1/5] drm/tests: Test drm_rect_intersect()](https://lore.kernel.org/r/4f21c74d-effd-1c04-d480-8c48de51bb54@riseup.net/)                                  |
| [Re: [PATCH v2 1/7] drm/vkms: isolate pixel conversion functionality](https://lore.kernel.org/r/1b04bc84-7c5d-6b8f-fa77-3407896d1dc7@riseup.net/)                      |
| [Re: [PATCH v3 1/6] drm/vkms: isolate pixel conversion functionality](https://lore.kernel.org/r/e988b4e1-978d-286c-77f7-9affc1cf7a3d@riseup.net/)                      |
| [Re: [PATCH v4 3/5] drm/tests: Add test cases for drm_rect_calc_vscale()](https://lore.kernel.org/r/a46b79c0-d134-6123-280e-c75a5f108e39@riseup.net/)                  |
| [Re: [PATCH v2 1/2] drm/vkms: allow full alpha blending on all planes](https://lore.kernel.org/r/f8ce92b5-962b-adef-284a-7254ba56321d@riseup.net/)                     |
| [Re: [PATCH v2 1/2] drm: Add fixed-point helper to get rounded integer values](https://lore.kernel.org/r/ffee1587-5236-ce35-40b4-5b8286dd095b@riseup.net/)             |
| [Re: [PATCH v2 2/2] drm/vkms: Fix RGB565 pixel conversion](https://lore.kernel.org/r/87fb8f63-ae38-33eb-08ef-7410b52b4f98@riseup.net/)                                 |
| [Re: [PATCH v3 1/2] drm: Add fixed-point helper to get rounded integer values](https://lore.kernel.org/r/7ac2cfb2-3912-675a-3ba0-171caab3ba30@riseup.net/)             |
| [Re: [PATCH 0/3] drm/vkms: Minor Improvements](https://lore.kernel.org/r/bc601be4-14e3-14f3-8d14-baea399150e2@riseup.net/)                                             |
| [Re: [PATCH v2] drm/vkms: Fix race-condition between the hrtimer and the atomic commit](https://lore.kernel.org/r/5ffa4aef-70eb-a2b9-b3e2-7ba00d706e16@riseup.net/)    |
| [Re: [PATCH] drm/tests: Add test case for drm_rect_clip_scaled()](https://lore.kernel.org/r/d813dbe5-4d9a-94d2-22f2-b480f68a8f6f@riseup.net/)                          |
| [Re: [PATCH v4] drm/vkms: Add support to 1D gamma LUT](https://lore.kernel.org/r/8c0e33e8-e95a-7c21-bc40-890ddfe05010@riseup.net/)                                     |
| [Re: [PATCH] drm/tests: Remove CONFIG_DRM_FBDEV_EMULATION on .kunitconfig](https://lore.kernel.org/r/bef940cb-b079-72ce-692c-3b6c6d283265@riseup.net/)                 |
| [Re: [PATCH] drm/tests: Remove CONFIG_DRM_FBDEV_EMULATION on .kunitconfig](https://lore.kernel.org/r/74e5cc8c-a5f8-4c0b-c699-4234e539203f@riseup.net/)                 |
| [Re: [PATCH] drm/tests: Alloc drm_device on drm_exec tests](https://lore.kernel.org/r/591114cb-88f7-0a43-f2ba-8ab5836571c9@riseup.net/)                                |
| [Re: [PATCH] drm: Drop select FRAMEBUFFER_CONSOLE for DRM_FBDEV_EMULATION](https://lore.kernel.org/r/7950bcea-0f15-da2e-e4f7-4bfa474a420f@riseup.net/)                 |
| [Re: [PATCH -next 6/7] drm/format-helper: Remove unnecessary NULL values](https://lore.kernel.org/r/28268a1b-090c-237a-79dd-ca58de712a1e@riseup.net/)                  |
| [Re: [PATCH 6/6] drm/format-helper: Add KUnit tests for drm_fb_memcpy()](https://lore.kernel.org/r/4cee9366-57c7-ec70-b547-072cc0a2d157@riseup.net/)                   |
| [Re: [PATCH v2 6/6] drm/tests: Add KUnit tests for drm_fb_memcpy()](https://lore.kernel.org/r/3dffe17d-4c6d-4cb7-2c22-c6d747e98bb0@riseup.net/)                        |

Moreover, I use a Thunderbird addon to make the diff properly
highlyted. When I was tinkering with the configuration, I noticed that
the CSS of the configuration menu was wrong, so it made the user
experience pretty bad.

I sent a [patch](https://github.com/Qeole/colorediffs/pull/116) fixing
that to the maintainer of the addon, this patch generated a discussion
that made a whole change in the CSS file due to Thunderbird updates.

# Acknowledgments

I'd like to thank my mentors, [André “Tony” Almeida][tony], [Maíra
Canal][maira], and [Tales L. Aparecida][tales]. Their support and
expertise were invaluable throughout this journey.

Moreover, I thank the X.Org Foundation for allowing me to participate
in this program, and also for accepting my talk proposal on the [XDC
2023](https://indico.freedesktop.org/event/4/contributions/179/).

Lastly, I thank my colleague Carlos Eduardo Gallo for exchanging
knowledge during the weekly meetings.

[tony]: https://andrealmeid.com/
[maira]: https://mairacanal.github.io/
[tales]: https://tales-aparecida.github.io/
