---
permalink: /
title: "About Me"
excerpt: "About Me"
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---
I work as a Programmer-Scientist at the [Douglas Mental Health University Institute](http://www.douglas.qc.ca/).
Academically, I hold an affiliate appointment with the [Department of Psychiatry at
McGill University](https://www.mcgill.ca/psychiatry/).
That job title however, doesn't tell you much about what I actually do, so I like to
describe it by explaining my many hats.

## The Systems Administrator and Devops Hat

I make sure the computers work. Servers, compute nodes, workstations, laptops and
all the miscellany that goes along with them. The Douglas Research Center
(and Cerebral Imaging Center, or CIC), has ~60 machines I'm responsible for.
I perform all the installation, setup and maintenance of this informatics system.
I install software people ask for, and make sure it works everywhere.

We use [Ubuntu Linux](http://ubuntu.com/), 
[ZFS](https://github.com/openzfs/zfs)+[NFS](https://en.wikipedia.org/wiki/Network_File_System),
and [SGE](https://arc.liv.ac.uk/trac/SGE) (soon [SLURM](https://slurm.schedmd.com/documentation.html))
to provide a uniform platform providing storage and compute across a heterogenous set of hardware.

I write software and automation wherever I can can to help me (see projects),
bash scripts, python scripts, ansible playbooks. I report bugs, and program fixes
to problems and contribute them back to the community when I can. We maintain a
suite of scientific software with the environment-modules (soon to be lmod) system.
This takes little of my day-to-day, because the computer hardware is reliable,
and I've automated much of the work.

From time to time, we need to deploy publicly facing software, I've worked with
[apache](https://httpd.apache.org/), [nginx](https://www.nginx.com/), [postgresql](https://www.postgresql.org/)
and [mariadb](https://mariadb.org/) in the cloud to deploy various platforms, and
I can successfully configure a standards-compliant mail server, even if Outlook365
silently drops that mail because Microsoft are jerks.

This compute platform used to be exclusively for the neuroimaging department of
the Douglas Research Center, but recently I became project lead of the Douglas
Neuroinformatics Platform, which will expand the platform to serve all of the Douglas
Research Center.

## The Scientific Programmer and HPC Hat

I write software for magnetic resonance imaging (MRI) processing, primarily for
structural imaging. Image registration is my favourite focus (see posts), but
I also work on classification and segmentation problems, see projects. I'm particularly proud
of my work in generalizing tools to work across a large range of species, from mice
all the way through non-human primates, to humans. I code in whatever is convenient,
bash, python, C, C++, perl and rarely, MATLAB. As you might have guessed, I'm not
an expert programmer in any of these languages, but rather an expert generalist.
I can learn what I need to, when I need to, to get solve problems, and so my expertise
and understanding grows slowly.

When I'm not programming my own software, I'm also helping others write their own within
the Douglas Research center, and helping them take existing software and get the most out
of the high-performance computing infrastructure we have available.

As part of the problem solving, I contribute to many public open source projects to fix
bugs, improve performance, and add new features. I have contributed to projects such as
[ITK](https://itk.org/), [bpipe](https://bpipe.org/), [ANTs](https://github.com/ANTsX/ANTs),
[RMINC](https://github.com/Mouse-Imaging-Centre/RMINC), [fMRIPrep](https://fmriprep.org/). 
In addition, I am one of the maintainers of the [MINC family](https://bic-mni.github.io/) of neuroimaging software.

## The Scientist Hat

I write my software with scientific questions in mind. I'm interested in a few things

- how to handle scanner, protocol and site variation using image preprocessing
- the impact quality (and motion corruption) has on the outcomes for neuroimaging studies
- the insights we can gain regarding neuropsychiatric disease comparing the human and non-human-primate brain structure and population-level variability

## The Educator Hat

When I'm not directly driving computers, I'm helping others to learn to do so.
I teach statistics, image processing, high performance computing, and programming
to the labs I support at the Douglas Research Center and worldwide collaborators.

I'm also a maintainer of the [shell-novice](https://github.com/swcarpentry/shell-novice/) lessons from
[Software Carpentry](https://software-carpentry.org/), an organization who's providing excellent
education materials to fill the computational knowledge gap in undergraduate and graduate science education.

