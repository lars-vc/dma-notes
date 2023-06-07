# DMA
## Chapter 8 Motion prediction
- frame differencing vs MCP
    - ME
    - MC
    - MCP/DF, DFD

forward/backward/bidirectional prediction

- motion estimation issues
    - motion model
    - matching criterion
    - region of support
    - search range
    - search strategy

3 cause of motion
- global motion (camera)
- local motion  of objects
- illumination variations

Models:
- affine/higher order models
    - affine
    - bilinear
    - perspective
- node mesh

Mostly used: translational only models

Something about frequency domain idk

BMME (high correlation of MV of neighboring blocks)
- BDM
    - SAD: |org - pred|
    - SSD: (org - pred)^2
- Effect of block size, smaller = better qual, but more data
- Effect of search range, larger = better qual, but longer compress time

motion failure
Problems
    - DFD has significant artifacts
    - DCT performs poorly
Solutions
    - intra vs inter
    - sub pel ME
    - in loop deblocking

Reduce complexity ME
- reduce complexity BDM
    - e.g. don't evaluate all pixels
- sub-sampled block motion field
    - ME on a subset of blocks
- hierarchical search techniques (refine coarse to finer)
- reduce set of MV candidates 
    - 2d log search
    - N setup search (NSS)
    - diamond search
    - hexagonal search (HEXBS)
- intelligent init and termination
    - reduced complexity ME might get stuck in local minima, so pick good starting point
    - put threshold on stopping so speedup BMME

Skip mode

Merge mode

MVP predict motion vectors based on neighboring motion vectors (think intra prediction style)


## Chapter 9 Hybrid Block based Video Codec
Combining block based ME with block based transform coding
I frame/slice: intra coded
P frame/slice: inter coded, 1 ref 
B frame/slice: inter coded, 2 ref 
IDR frame: any refs can not go beyond this frame

Intra prediction

Sub pixel ME
- 1/8 of pixel best bang for buck
Approaches
- interpolate current block and search window, then ME
- interpolate only the search window, then ME
- integer-pixel ME, then refine with interpolated block and search window
- integer-pixel ME, then refine with interpolated search window

MRF-ME

Variable block size for ME

Variable sized transforms (recently smaller transforms are chosen, less ringing) \
Drift with DCT and IDCT precision

In loop deblocking operations
Blocking artifacts may arise because
- ME
- transform coding of the difference signal
Determine if
- edge is part of image
- edge is induced by coding
Filtered image is used for MCP of future frames (in loop)
- can improve compression
- filtered image is usual more faithful to the original then block unfiltered image

## Chapter 10 Measuring quality

QoE

Influence factors:
- Human IF: property of a human user. The characteristic can describe the demographic and socioeconomic background, the physical and mental constitution, or the users emotional state
- System IF: Technically produced quality of an application or service. They are related to media capture, coding, transmission, storage, rendering, and reproduction/display, as well as to the communication of information itself from content production to user
- Context IF: Situational property to describe the user’s environment in terms of physical, temporal, social, economic, task, and technical characteristics

Sobel filtering

Testing
- test material
- test conditions
- test subjects
- test environment
- test methodology

Scales
- DSCQS
- DSIS
- ACR/ACR-HR

Problems
- Central tendency bias
- Acquiescence bias
- Disagree with sentences as presented out of a defensive desire to avoid making erroneous statements
- Faking good
- Faking bad

Objective vs Subjective testing

MOS/DMOS

4k sensor don't always shoot 4k details

Acuity of the eye

- Linear correlation
- Rank order correlation
- Outlier ratio

Why use objective metrics:
- algorithm development and benchmarking
    - subjective testing too cumbersome
    - simple means of comparison
    - guidance regarding benefit of algorithmic modifications
- rate-distortion optimization
    - quality assessments in encoding loop
- streaming control

Type of objective metrics
- FR (full reference)
    - we have original material
    - e.g. PSNR
- NR (no reference)
    - original material not available
    - generally restricted to specific scenarios and distortion types (e.g. blur)
- RR (reduced reference)
    - partial info about source
    - use features from the original content and the distorted content

Types in different dimension:
- Pixel based
- Stream based
- Hybrid

Examples
- PSNR
- SSIM
- VMAF
    - VIF
    - DLM
    - Mean Co-Located Pixel Difference
- VSNR
- VQM


### RDO
Difficulties
- Size of parameter space
- Best setting vary for different spatio-temporal regions
- RD costs are not independent of all coding units

Convex hull
Lagrangian optimization

In practice
- separate RDO evaluation for each coding unit/parameter
- optimization/early termination strategies

Rate Control
Ensures that the coded video is delivered to the channel at a rate compatible with the prevailing content complexity and channel capacity Without rate control, decoder buffers would regularly underflow or overflow, resulting in playout jitter or loss
VBR/CBR
Problem when combining RDO and rate control
- To perform RDO for a coding unit → QP must be known
- To do rate control → QP depends on the content and thus on SAD or a variance measure.
- The actual SAD of the coding unit is not available until after the RDO process is complete;
    - Rate control estimates the SAD of the current coding unit
- Header information such as coding unit modes and MV information are not
available before the RDO process completes;
    - Rate control needs to guess target bits for the current frame/coding unit

Rate constraint
HRD
2 pass encoding

ROI

## Chapter 11 Delivery Across Networks
General approach
- understand causes of errors, and their effect
- characterize these w.r.t. HVS
- use appropriate channel coding (and/or retransmission strategies)
- apply joint source-channel coding or cross-layer solutions
- use video coding tools to make the encoded bitstream inherently more robust (application layer)
- optimally conceal errors in reconstructed video

Reasons for spatial error propagation
- loss of VLC sync (i.e., incorrect superposition of DCT basis functions)
- loss of block sync (i.e., incorrect EOB symbols)
- DPCM coding (e.g., DC coefficients in JPEG)
- Intra prediction (e.g., H.264/AVC Intra)

Temporal error propagation

Rate distortion performance vs error resilience

Transport layer solutions
- ARQ
- FEC 
- Packetization solutions

Frame type (I – P – B)
- I frames are barriers for temporal error propagation
    - but may introduce spatial errors
- P frames propagate errors from reference frames
    - and may introduce new spatial errors
- B frame are generally not used as reference
    - errors are contained within a frame

Intrarefresh
Reference Picture Selection
Periodic Reference Frames

Synchronization codeword

Reversible VLC

Slice Structuring
- FMO ||Flexible macroblock ordering||
- Redundant slices
- Data partitioning

EREC

PVQ ||Pyramid Vector Quantization||

### Error concealment
Detect errors
- header information
- error detecting/correcting codes
- erroneous syntax or semantics
- deviating video signal characteristics

Spatial error concealment
- weighted average
- sobel filter for edge detection

Temporal error concealment
- temporal copying
- motion compensated temporal replacement
    - estimation of displacement
    - evaluation
- estimation of the displacement (data and motion vector is lost)
    - MV from spatial neighbors
    - MV from temporal neighbors
    - combination
    - (external) Boundary matching error

Hybrid methods

### Congestion management and scalable video coding
SVC (multi loop) = hierarchical structure of dependent coding layers
MDC = number of independent ‘encodings’ (descriptions)


## Chapter 12 Video Coding Standards

4 stages + PPPCEAP
Learn entire table


## Chapter 13 The future
(note slides B first then A)
360 video
- ERP
    - Downsides:
    - Pixel redundancies in uninteresting regions
    - Seam where left and right are stitched together
    - Object follow less linear motion
    - Objects deform
- EAP
    - solves oversampling at poles
- CMP
- SSP

Problems with 360 video
- Regular 360 video (1 sphere) Head doesn't rotate around vertical axis of eye
- Stereoscopic camera (2 spheres) Gradually decreasing stereo effect towards 90 angle, cameras will capture same image when looking to the side 90 degrees
- Stereoscopy
    - VAC

### Light fields
Capturing
- Dense light field
- Sparse light field

Light field rendering, cna be performed after capturing!
5D light field to 4D light field
Light slab = focal plane + camera plane
Inherent to the light field:
- walk around
- choosing aperture
- choosing focus distance
Application of the light field:
- 4D depth processing
- 4D camera tracking
- CGI integration

### Light field encoding
Inter prediction approach (snake): not good because you usually only need a part of the image but you still need to decode the whole thing
Multiview video compression approach (matrix): same problem but better compression
Multiview + depth: send less but add some depth info, so this introduces loss but is more accessible (everything become matt, lambertian)

### Other stuff
Spatial resolution

Temporal resolution

Dynamic range
- stops => double brightness == bits
- HDR

Barten ramp (15bit -> 12 bit)

WCG


## Chapter 14 3D video encoding
Textures vs 3D models
Piecewise parametric vs piecewise implicit
manifolds
- compatible, orientable, genus

mesh
- connectivity
- geometry
- attributes

LOD

### Quality metrics
Hausdorff
Critical thoughts
- circle, size vs crumpled
- pov matters

### Quantization
traditional quantization
Deering
Touma Gotsman


## Chapter 15 Audio compression
### Psychoacoustics
Threshold of hearing
Equal loudness relations (fletcher munson curves) phons
Frequency masking
Critical bands
Temporal masking

