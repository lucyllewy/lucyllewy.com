---
title: "Virtualisation"
date: 2015-10-27 21:09:23
published: true
categories: [sysadmin]
wpid: 726
---

When it comes to day-to-day computing Dani likes to use many operating systems depending on his mood and what they currently have installed. Usually Dani will just go with whatever is currently running on his desktop computer and run with that. However, this often leaves him feeling somewhat constrained by whichever operating system was active when they last left the machine and let it go into low-power mode as they try to maintain state between computing sessions – so that they can pick up on things they left days before.

What Dani really needs, and there is now a small project\* which seems to be a possible solution, is for a bare-metal hypervisor like XEN. This is a problem, though, as XEN usually requires a full operating system running in the so-called Dom0 through which the hypervisor is controlled. The operating systems installed as Dom0 are either a full-blown desktop OS or a limited "headless" system designed to be administered over the network for server workloads such as Citrix's XenServer product.

*\* The project is [Qubes-OS](https://qubes-os.org/), and is primarily built with security in mind.*

Dani really wants the power of a bare-metal hypervisor with a cut-down Dom0 OS which has a usable GUI for running desktop workloads on the DomU's (guest systems in VMWare speak). Qubes seems to be the closest system to achieving this goal, however there are a few caveats such as OpenGL being unsupported in the DomU's. (Note, there is a project, which appears to have been abandoned, with aims to provide OpenGL capabilities to virtual machines called [VMGL](https://web.archive.org/web/20101010094652/http://www.cs.toronto.edu:80/~andreslc/xen-gl/))

Bottom line? Dani wants something akin to:

- Citrix XenServer or VMware ESXi with a desktop GUI instead of the headless system currently employed.
- Cut and Paste between VMs must be supported.
- Hardware accelerated graphics, wherever possible. (Including DirectX and OpenGL.)
- Unity<sup>© VMWare Inc.</sup>-esque seamless desktop operation for *all* virtual machines by default.
- AIGLX compositing on the Dom0 
  - if the devs feel they *must* enable the pointless and horrid wobbly windows etc. features then provide a way of turning that off (like ubuntu with it's three-step design of "off", "minimal" and "maximal" where only maximal has stupid crap like wobbly windows).
- Windows support as standard, unlike Qubes which seems to want to make that a premium feature.
- Free for personal, and/or single system, use like Virtualbox and current XenServer & ESXi licenses OR reduced pricing for personal use over business/corporate use. 
  - Extra features can be enabled in the corporate version such as desktop provisioning, kill switch, and similar corporately-desired facilities which are pointless in a domestic situation.
- Ability to have certain VMs bootup with the host like XenServer, VMware Server and ESXi currently allow.