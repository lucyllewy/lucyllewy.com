---
title: "Linode and Corosync High Availability"
date: 2010-11-17 00:52:12
published: true
categories: [sysadmin]
wpid: 771
---

I have recently been configuring multiple [Virtual Private Servers, hosted at Linode.com](https://www.linode.com/?r=c56c84cd2209eca9eb13480f9d445fd90664c18e)'s UK data-centre, into a highly available (HA) web-hosting and IRC cluster. My design required the use of a shared storage using DRBD, which is akin to network-based RAID Level 1 or mirroring. To allow two servers to be operational simultaneously while maintaining data consistency DRBD can operate in a Primary/Primary configuration instead of the usual master-slave scenario; the data is kept consistent by using the OCFS2 file-system over the top of the DRBD device. The idea is that DRBD maintains the raw data in sync, while the OCFS2 file-system maintains file locks and related technologies in sync between the two servers preventing both from writing to the same file simultaneously.

Because of my use of OCFS2, which requires a Distributed Lock Manager to operate successfully, and that I wanted the file-system to fail-over nicely meant that I could not use heartbeat as my High Availability back-end to Pacemaker the cluster resource management framework. The preferred HA system for Pacemaker is Corosync, so I installed this from Ubuntu's Apt repository and started the cluster.

Unfortunately neither system would recognise that the other existed when configured to use the default multicast addressing system. (Multicast was chosen to allow each system to "subscribe" to the data feed and other systems, those not in the cluster, wouldn't get inundated with irrelevant information.) While researching this problem I found posts on Linode's forum website that suggested multicast was allowed along with broadcast. This turns out not to be the case, as pinging 192.168.255.255, the broadcast address for my private network at Linode, yielded absolutely no replies from any of my other systems. This confirmed that Broadcast was not allowed, and added to my suspicion that Multicast would be the same.

So then I started searching for Corosync and Unicast to try to find a solution that allowed the HA daemon to directly communicate with all members of the cluster on a pre-arranged basis rather than relying on automatic discovery. I found a patch on the Corosync mailing list which does just that, utilising UDP. (The relevant term for google is "UDPU".) Unfortunately, again, the compilation process was not easy.

Howto build Corosync
--------------------

To compile Corosync, a couple other packages are required to have also been compiled (step-by-step below). Also the packaged version of Pacemaker from Ubuntu won't work with the custom compilation of Corosync, so we need to compile that too.

### Install the Build Tools

```bash
sudo apt-get install build-essential
sudo apt-get install groff # I forget where this was needed now, but at some point it'll complain if you don't have it.
```

### Cluster Glue

#### gatomic.h.patch

```diff
*** gatomic.h.old    2010-11-16 18:57:22.000000000 +0000
--- gatomic.h    2010-11-15 05:14:23.000000000 +0000
*************** void     g_atomic_pointer_set
*** 64,79 ****
#else
# define g_atomic_int_get(atomic) 
((void) sizeof (gchar [sizeof (*(atomic)) == sizeof (gint) ? 1 : -1]), 
!   (g_atomic_int_get) ((volatile gint G_GNUC_MAY_ALIAS *) (void *) (atomic)))
# define g_atomic_int_set(atomic, newval) 
((void) sizeof (gchar [sizeof (*(atomic)) == sizeof (gint) ? 1 : -1]), 
!   (g_atomic_int_set) ((volatile gint G_GNUC_MAY_ALIAS *) (void *) (atomic), (newval)))
# define g_atomic_pointer_get(atomic) 
((void) sizeof (gchar [sizeof (*(atomic)) == sizeof (gpointer) ? 1 : -1]), 
!   (g_atomic_pointer_get) ((volatile gpointer G_GNUC_MAY_ALIAS *) (void *) (atomic)))
# define g_atomic_pointer_set(atomic, newval) 
((void) sizeof (gchar [sizeof (*(atomic)) == sizeof (gpointer) ? 1 : -1]), 
!   (g_atomic_pointer_set) ((volatile gpointer G_GNUC_MAY_ALIAS *) (void *) (atomic), (newval)))
#endif /* G_ATOMIC_OP_MEMORY_BARRIER_NEEDED */

#define g_atomic_int_inc(atomic) (g_atomic_int_add ((atomic), 1))
--- 64,79 ----
#else
# define g_atomic_int_get(atomic) 
((void) sizeof (gchar [sizeof (*(atomic)) == sizeof (gint) ? 1 : -1]), 
!   (g_atomic_int_get) ((volatile gint G_GNUC_MAY_ALIAS *) (volatile void *) (atomic)))
# define g_atomic_int_set(atomic, newval) 
((void) sizeof (gchar [sizeof (*(atomic)) == sizeof (gint) ? 1 : -1]), 
!   (g_atomic_int_set) ((volatile gint G_GNUC_MAY_ALIAS *) (volatile void *) (atomic), (newval)))
# define g_atomic_pointer_get(atomic) 
((void) sizeof (gchar [sizeof (*(atomic)) == sizeof (gpointer) ? 1 : -1]), 
!   (g_atomic_pointer_get) ((volatile gpointer G_GNUC_MAY_ALIAS *) (volatile void *) (atomic)))
# define g_atomic_pointer_set(atomic, newval) 
((void) sizeof (gchar [sizeof (*(atomic)) == sizeof (gpointer) ? 1 : -1]), 
!   (g_atomic_pointer_set) ((volatile gpointer G_GNUC_MAY_ALIAS *) (volatile void *) (atomic), (newval)))
#endif /* G_ATOMIC_OP_MEMORY_BARRIER_NEEDED */

#define g_atomic_int_inc(atomic) (g_atomic_int_add ((atomic), 1))
```

#### Procedure

```bash
sudo apt-get install libltdl-dev libglib2.0-dev libxml2-dev libbz2-dev intltool
sudo ln -s libuuid.so.1 /lib/libuuid.so
cd /usr/include/glib-2.0/glib
sudo patch -Np5 < gatomic.h.patch
cd ~
wget -O cluster-glue.tar.bz2
tar jxvf cluster-glue.tar.bz2
cd Reusable-Cluster-Components-*
./autogen.sh
LDFLAGS="-L/lib" LIBS="-luuid" ./configure --prefix=/usr --with-daemon-user=hacluster --with-daemon-group=haclient
make
sudo make install
cd ..
```

### Resource Agents

```bash
wget -O resource-agents.tar.bz2
tar jxvf resource-agents.tar.bz2
cd Cluster-Resource-Agents-*
./autogen.sh
./configure --prefix=/usr
make
sudo make install
cd ..
```

### OpenAIS

```bash
sudo apt-get install subversion
svn co https://svn.fedorahosted.org/svn/openais/branches/wilson
mv wilson openais
cd openais
./autogen.sh
./configure --prefix=/usr --with-lcrso-dir=/usr/libexec/lcrso
make
sudo make install
cd ..
```

### Corosync

[udpu.patch_.zip](/blog/linode-and-corosync-high-availability/files/udpu.patch_.zip)

```bash
sudo apt-get install git-core pkg-config
git clone git://corosync.org/corosync.git
cd corosync
unzip udpu.patch_.zip
git apply udpu.patch
./autogen.sh
./configure --disable-nss
make
sudo make install
```

### Pacemaker

```bash
apt-get install libxslt-dev
wget -O pacemaker.tar.bz2
tar jxvf pacemaker.tar.bz2
cd Pacemaker-1-0-*
./autogen.sh
./configure --prefix=/usr --with-lcrso-dir=/usr/libexec/lcrso
make
sudo make install
```

I will also be posting an article soon about getting DRBD, OCFS2 and MySQL Multi-Master (aka MySQL Master/Master) working with the new Pacemaker system.