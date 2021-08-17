---
title: "Non linear registration doesn't matter (much) or how I learned to stop worrying and love the affine"
date: 2019-06-04
permalink: /posts/2019/06/Non-linear-registration-doesnt-matter-much-or-how-I-learned-to-stop-worrying-and-love-the-affine
tags:
  - mri
  - image registration
---

![](https://upload.wikimedia.org/wikipedia/commons/b/bb/Dr._Strangelove.png)

## Introduction

In this document, I want to go over the goals of a registration for MAGeTbrain vs brain-to-brain registration which is typically done with antsRegistration or ANIMAL.

MAGeTbrain's ultimate goal is to propagate a set of labelled anatomical regions onto a set of brains which are not anatomically labelled. In comparison, a typical brain-to-brain registration has the goal of achieving point-to-point correspondence across all of the anatomy. These goals are at first glance aligned; to achieve labelling, one wishes to create point-to-point correspondence for the labelled anatomy. However, as we argue here, in real world cases, this correspondence of goals rapidly breaks down.

In each of the following sections, I will go over an example of how changing the configuration of the initial affine registration, either through masking, or adjusting the type input file provided, can have a substantial impact on the resultant segmentation.

## Non-Linear Registration's Importance
Much ado is made about the relative performance of different non-linear registration algorithms, with the ANTs SyN with local cross-correlation generally being considered  the best (as of 2018). While this may be the case, no non-linear registration can compensate for a poor affine initialization. Tissues that barely overlap to start are unlikely to be sufficiently deformed to achieve anatomical correspondence. As such, the majority of this document highlights the importance of getting initialization right to begin with.

## Skulls
Consider the case of two subjects, one healthy young adult within their 20's (like the MAGeTbrain high-res atlases) and one with mild alzheimer's disease (AD) in their 70's. If we further postulate that these individuals are of the same sex, socioeconomic background, and ethnicity, we could, for argument’s sake, conceive that their skull shapes and size might be very comparable, or even nearly identical. Now, we have a case where the skull-based contrasts and information are identical between subjects, but we expect that the brain, particular the cortex of the aged AD subject, will be substantially degenerated, as well as likely having a corresponding increase in ventricular volume, rendering a cerebrospinal fluid (CSF) enlargement within the skull cavity. Given that an affine alignment only has a total of 12 parameters by which to achieve a minimization of the cost function, a low-error solution to this registration does not exist. Any registrations between these two brain scans as-is will necessitate a tradeoff in the affine alignment, between the skull, brain, and CSF cavities.

![image](https://user-images.githubusercontent.com/3001850/58187322-89be7b00-7c84-11e9-87ac-d8b3e09b71af.png)

**As such, it is best practice when using MAGeTbrain to supply skull-stripped scans for all inputs.**

## Cortex and Ventricles
As mentioned in the discussion above, in addition to a reduction in the cortex volume, there is often an increase in ventricular volume in addition to, or instead of shrinkage of the cortex. After the removal of the skull from scans, we now have a new conflict: the affine registration will have a strong bias to match the overall size and shape of the brain as a whole due to the very hard edge contrast from the skull stripping. As a result, brains with different ratios of tissue to internal CSF, most often seen in older subjects or those with neurodevelopmental disorders, will have brains that still differ overall from a normal healthy scan such as those that are used to make our atlases. This conflict will again result in the affine optimization settling on a compromise between satisfying the internal structural and the outer edge contrast, usually resulting in a bias towards the other edge. This bias likely results in the neuroanatomical region of interest not corresponding well.

![image](https://user-images.githubusercontent.com/3001850/58187780-73fd8580-7c85-11e9-9437-7d0a26a2dc4c.png)

How does one overcome such biases? To do so, we must consider exactly which neuroanatomical area we are actually interested in segmenting. Drawing an imaginary horizontal plane through a subject's brain, we can visualize that the core brain structures of which MAGeTbrain currently offers segmentations, all exist below a plane that cuts through the ventricles and below the cortex (i.e. the striatum, globus pallidus, thalamus, hippocampus, and cerebellum). This means that in any given brain, the upper edge of the ventricles and even most of the cortex are irrelevant for the overall goal of propagating labels. We can take advantage of this fact by using the labels of the atlases (merged and padded appropriately) as masks to focus the affine registrations. In doing so, we can make it so that the affine registration only aligns the target anatomy, which would particularly improve the accuracy of segmentation in cases where subjects have extensive ventricular enlargement or very differently shaped cortex lobes.

**As such, the default MAGeTbrain implementations now use the input labels provided with the atlases as a mask to target the registrations.**

## Thy Fearful Symmetry
The classic MNI model that most people use is the icbm_sym_09c model, that is, its left-right symmetric. You would be hard pressed to notice the difference if I swapped it out for the icmb_aysm_09c model. Normal young healthy people have highly symmetric brains, with a very slight "twist" called the Yakovlevian torque. However, as previously referenced, what holds for healthy young brains often does not for those afflicted by neurodevelopmental, degenerative, or psychiatric afflictions. Take for example semantic dementia, where degeneration is entirely focal and localized to the left temporal cortex. The tissues adjacent to the left temporal cortex, the left hippocampus most acutely, are shifted laterally outwards compared to their typical anatomical locations. Meanwhile, the structures on the right side are still in their typical locations (relative to the midline).

![image](https://user-images.githubusercontent.com/3001850/58187958-d6568600-7c85-11e9-8c1e-f76706cdd24c.png)
The MNI symmetric (left) and asymmetric (right) models. Note the slight "twist" in the occipital lobe.

Now, if, as recommended above, we skullstrip the brains of two subjects (one young healthy person and one with semantic dementia), and use the typical MAGeTbrain label sets to focus the registration, we arrive at a new problem: an affine registration cannot simultaneously align both the left and right hippocampi from the atlas onto the target scan, as any attempt at scaling will move both of them equally, and any shift will similarly move both, so we must arrive at some sort of registration compromise.

How do we deal with this problem where our target brain is asymmetric but our source labelled brain is symmetric? We break the symmetry. The label masking infrastructure built into MAGeTbrain is agnostic to the labels provided. This means we can split our label set up into left and right labels, and provide them to separate MAGeTbrain runs. Now, the symmetry of the original labelset is broken; effectively, we have doubled the degrees of freedom for each set, allowing them the ability to align with their target anatomy, further relaxing the registration requirements everywhere else. 

Based on the arguments above, we recommend that for any population with signs of asymmetry, which includes most patient populations of interest, that you run MAGeTbrain with separate left/right runs.
What about structures on the midline?
The cerebellum in particular is the ‘odd man out’. While it has left-right structures and may have asymmetry, it is also a single contiguous structure without unrelated tissue in between, at least as defined in our label sets. It does however provide strong exterior edge anchor points for affine registrations if included in a registration. It is also much larger than other structures, meaning that it can dominate the statistics of image registration due to large voxel counts.

**Based on this, we recommend that when splitting the MAGeTbrain runs left/right, the cerebellum label set be further split into a seperate run.**

### How much splitting is too much?
So far, every argument for changing the way we setup registrations has suggested that we split the MAGeTbrain runs into smaller pieces, but how much is too much? The ultimate limit for splitting is label adjacency. Part of MAGeTbrain's success has been its ability to maintain the relative proportions and relationship of adjacent anatomy through the SyN Gaussian regularization in ANTs, if the subfields of the hippocampus were split up, there is no longer a guarantee in a given registration run that subfields would not cross into adjacent subfields. One could conceivably split each left/right anatomically coherent label into its own MAGeTbrain run, but only in the most extreme circumstances would that be needed.

## So what should I do?
What does this mean for running MAGeTbrain? Well, this depends on your population, but first, the general recommendations.

Use extracted brains for all templates/subjects
This is ``*beastextract.mnc`` from ``minc-bpipe-library``, which you should be using for your preprocessing
``/project/m/mchakrav/atlases_from_git/{T1,T2}_extracted`` contain extracted atlases
Use labelmasking for your MAGeT registration
This is the default on the ``simplified-labelmask`` and ``morpho-labelmask``

For populations which have neurodegeneration, or strong asymmetry, or both, split your MAGeTbrain runs into three seperate runs:
1. Left
     - Left hippocampus
     - Left amygdala
     - Left striatum
     - Left globus paladus
     - Left thalamus
2. Right
     - Right hippocampus
     - Right amygdala
     - Right striatum
     - Right globus paladus
     - Right thalamus
3. Cerebellum

You can find left/right split labels at ``/project/m/mchakrav/atlases-leftright/merged-5atlas-labels/labels``
