===========================================
0. Downloading and Saving Your Initial Data
===========================================

We're going to run transcriptome assembly completely in the cloud,
because that way (a) you don't need to buy a big computer, and (b)
I don't have to figure out all the special details of your own
computer system.

This does mean that the first thing you need to do is get your data
over to the cloud.  I tend to just store it there in the first place,
because...

The basics
----------

... Amazon is happy to rent disk space to you, in addition to compute time.
They'll rent you disk space in a few different ways, but the way that's
most useful for us is through what's called Elastic Block Store.  This
is essentially a hard-disk rental service. 

There are two basic concepts -- "volume" and "snapshot". A "volume" can
be thought of as a pluggable-in hard drive: you create an empty volume of
a given size, attach it to a running instance, and voila! You have extra
hard disk space.  Volume-based hard disks have two problems, however:
first, they cannot be used outside of the "availability zone" they've
been created in, which means that you need to be careful to put them
in the same zone that your instance is running in; and they can't be shared
amongst people.

Snapshots, the second concept, are the solution to transporting and
sharing the data on volumes.  A "snapshot" is essentially a frozen
copy of your volume; you can copy a volume into a snapshot, and a
snapshot into a volume.

Getting started
---------------

You only have to run through this once.  Essentially you create a disk; attach it; format it; then copy things
to and from it.

Downloading and saving your data to a volume
--------------------------------------------

Got to NCBI `PRJNA231566 SRA Bioproject  <http://www.ncbi.nlm.nih.gov/sra?linkname=bioproject_sra_all&from_uid=231566>`__ 

Create and download a .csv spreadsheet file:

.. image:: ScreenShot_2015-10-04.png

Then, you will have to upload it to your instance:

.. code:: 

   rsync -e "ssh -i [key].pem" -avz [source directory] [user]@[instance ip]:[destination directory on instance]






Once you have the files, figure out their size using ``du -sh`` (e.g. after the
above, ``du -sh /mnt`` will tell you how much data you have saved under /mnt),
and go create and attach a volume (see :doc:`../amazon/index`).

Any files in the '/mnt' directory will be lost when the instance is stopped or
rebooted. However, files stored in the root, '/', directory will remain
available. Thus, it's a good rule of thumb to do "savepoints" -- whenever you
complete a big chunk of work, think about saving the data at that point.  I've
broken the mRNAseq tutorial down into chunks of work whereyou can do this --
after each Web page, basically. To sync a folder to attached volume simply
type::

   rsync -av folder_to_keep /path_to_volume

Some test data
--------------

data is on snapshot 'snap-f5a9dea7', so go create a volume from that
and mount it as '/data' to get started; to mount it read-only, do::

   mount -o ro /dev/xvdf /data

after attaching the volume as 'sdf'. 

Additional information
----------------------

Throughout this protocol we will be using commandline interfaces. There
is a short document explaining the notations used here. (see :doc:`../docs/command-line`)

----

Next: :doc:`1-quality`
