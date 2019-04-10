## Neural mass model with CMC and NMDA parameters

In the model, you will find the following substruct multiple times, which will be called `model_definition`. 
This set of parameters needs to appear in the priors and constitutes, for instance, the posterior estimate substruct `DCM.Ep`.
`n` stands for the number of sources which are modelled.

```
Ep
├─── S                              # population variance
├─── T: [n×3 double]                # rate constants for AMPA, GABA and NMDA channels
├─── G                              # intrinsic connectivity
├─── GN_intrin: [n×1 double]        # input scale to NMDA for each of the n sources. Pyramidal NMDA?? Superficial/deep?
├─── GA_intrin: [n×1 double]        # input scale to AMPA for each of the n sources.
├─── GG_intrin: [n×1 double]        # input scale to GABA for each of the n sources.
├─── GNi_intrin: [n×1 double]       # input scale to interneurons NMDA
├─── GAi_intrin: [n×1 double]       # Mg-switch sigmoid scale, slope and sensitivity 
├─── GGi_intrin: [n×1 double]       # input scale to GABA for each of the n sources
├─── CV: [1×n double]               # membrane capacitance for all n sources
├─── E                              # background noise
├─── A                              # extrinsic connections for AMPA (forward {1} and backward {2} connections between sources, row: to, col: from)
├─── AN                             # extrinsic connections for NMDA (same shape as A)
├─── C                              # subcortical input
├─── H                              # intrinsic connectivity. Unused?
├─── R                              # onset and dispersion.
├─── D                              # delays (unused)
├─── Lpos                           # ROIs (unused)
├─── L                              # leadfield
├─── J                              # contributing states
├─── a                              # neuronal innovations?
├─── b                              # channel noise (not source-specific)??
├─── c                              # channel noise (source-specific)??
├─── d                              # channel noise (basis set coefficients)??
```

And now, I give you the DCM struct:
```
DCM
├─── M
|    ├─── P                         # model parameters
|    |    ├─── model_definition
|    ├─── pE                        # prior expectation
|    |    ├─── model_definition
|    ├─── Nmax
|    ├─── dipfit                    # spatial model specification. Gives dipole structure??
|    |    ├─── model
|    |    ├─── type
|    |    ├─── Ic
|    |    ├─── location
|    |    ├─── symmetry
|    |    ├─── Lpos
|    |    ├─── modality
|    |    ├─── Ns
|    |    ├─── Nc
|    |    ├─── silent_source
|    |    ├─── vol
|    |    |    ├─── bnd
|    |    |    |    ├─── pos
|    |    |    |    ├─── tri
|    |    |    ├─── cond
|    |    |    ├─── mat
|    |    |    ├─── type
|    |    |    ├─── unit
|    |    |    ├─── cfg
|    |    |    |    ├─── method
|    |    |    |    ├─── outputfilepresent
|    |    |    |    ├─── toolbox
|    |    |    |    ├─── callinfo
|    |    |    |    ├─── version
|    |    |    |    ├─── headshape
|    |    |    |    ├─── conductivity
|    |    |    |    ├─── tissue
|    |    |    |    ├─── smooth
|    |    |    |    ├─── threshold
|    |    |    |    ├─── numvertices
|    |    |    |    ├─── isolatedsource
|    |    |    |    ├─── fitind
|    |    |    |    ├─── point
|    |    |    |    ├─── submethod
|    |    |    |    ├─── feedback
|    |    |    |    ├─── radius
|    |    |    |    ├─── maxradius
|    |    |    |    ├─── baseline
|    |    |    |    ├─── singlesphere
|    |    |    |    ├─── tissueval
|    |    |    |    ├─── transform
|    |    |    |    ├─── siunits
|    |    |    |    ├─── headmodel
|    |    |    |    ├─── previous
|    |    |    ├─── skin_surface
|    |    |    ├─── inner_skull_surface
|    |    |    ├─── source
|    |    ├─── datareg
|    |    |    ├─── sensors
|    |    |    |    ├─── chanpos
|    |    |    |    ├─── elecpos
|    |    |    |    ├─── chantype
|    |    |    |    ├─── chanunit
|    |    |    |    ├─── label
|    |    |    |    ├─── unit
|    |    |    ├─── fid_eeg
|    |    |    |    ├─── pnt
|    |    |    |    ├─── fid
|    |    |    |    |    ├─── pnt
|    |    |    |    |    ├─── label
|    |    |    |    ├─── unit:'mm'
|    |    |    ├─── fid_mri
|    |    |    |    ├─── pnt
|    |    |    |    ├─── fid
|    |    |    |    |    ├─── pnt
|    |    |    |    |    ├─── label
|    |    |    |    ├─── unit:'mm'
|    |    |    ├─── toMNI
|    |    |    ├─── fromMNI
|    |    |    ├─── modality: 'EEG'
|    |    ├─── sens
|    |    |    ├─── chantype
|    |    |    ├─── chanunit: {'V', ... , 'V'}
|    |    |    ├─── elepos
|    |    |    ├─── label
|    |    |    ├─── type
|    |    |    ├─── unit: 'mm'
|    |    |    ├─── chanpos
|    |    ├─── siunits
|    |    ├─── G
|    |    ├─── U
|    |    ├─── Ip
|    |    ├─── radius
|    |    ├─── Nm
|    |    ├─── Nd
|    |    ├─── gainmat
|    ├─── IS
|    ├─── FS
|    ├─── g
|    ├─── f: 'spm_fx_cmm_NMDA'
|    ├─── x
|    ├─── n
|    ├─── pC
|    |   ├─── model_definition
|    ├─── hE
|    ├─── hC
|    ├─── m
|    ├─── u
|    ├─── U
|    ├─── l
|    ├─── Hz
|    ├─── dt
├─── name: '<name of DCM file>'
├─── xY: [1x1 struct]                                   # data
|    ├─── Dfile: '<path/to/preprocessed/EEG/data>'
|    ├─── y
|    ├─── xy
|    ├─── modality: 'EEG'
|    ├─── name
|    ├─── Ic
|    ├─── Time
|    ├─── dt
|    ├─── coord2D
|    ├─── pst
|    ├─── It
|    ├─── nt
|    ├─── code
|    ├─── scale
|    ├─── X0
|    ├─── R
|    ├─── Hz
|    ├─── csd
|    ├─── orig_data_size
|    ├─── inds_used_size2
|    ├─── csd_per_trial
|    ├─── U
|    ├─── Q
├─── options
|    ├─── trials
|    ├─── analysis: 'CSD'
|    ├─── model: 'CMM_NMDA'           # 'ERP', 'SEP', 'CMC', 'LFP', 'NMM' or 'MFM'
|    ├─── spatial: 'IMG'              # 'ECD', 'LFP' or 'IMG'     (see spm_erp_L)
|    ├─── Tidcm                       # [start end] time window in ms
|    ├─── Fdcm                        # [start end] Frequency window in Hz
|    ├─── Nmodes                      # number of spatial modes???
|    ├─── D                           # time bin decimation       (usually 1 or 2)??
├─── Lpos
├─── Sname: {'S1', 'S2, ... }         # source names
├─── A                                # binary constraints on the extrinsic (between source) connections
├─── C:n×0 empty sparse double matrix # binary constraints on the regions which receive external input. In this case, we are collecting resting-state data, hence the zero.
├─── B                                # binary constraints on the modulatory connections for each of the m conditions.
├─── xU                               # design
|    ├─── X
├─── val
├─── dtf                              # directed transfer functions (source space)
├─── ccf                              # cross covariance functions (source space)
├─── coh                              # cross coherence functions (source space)
├─── fsd                              # specific delay functions (source space)
├─── pst                              # peristimulus time
├─── Hz                               # frequency (vector with integer valuesof the analyzed frequency spectrum, e.g. if 2-10 Hz, then DCM.Hz is the same as the vector [2:10].
├─── Ep                               # conditional expectation. Meaning your posterior estimates of the model parameters??
|    ├─── model_definition
├─── Cp                               # conditional covariance. posterior covariance matrices??
├─── Pp                               # conditional probability. posterior probability of each parameter??
|    ├─── model_definition
├─── Hc                               # conditional responses (y), channel space. Model estimates for the generated data??
├─── Rc                               # conditional residuals (y), channel space. Residuals??
├─── Hs                               # conditional responses (y), source space
├─── Ce                               # eML error covariance
├─── Ce_Eh
├─── F                                # Laplace log evidence
├─── ID                               # data ID

```

## Resources:
- [spm_dcm_csd.m](https://github.com/spm/spm12/blob/master/toolbox/dcm_meeg/spm_dcm_csd.m)
- [spm_erp_L.m](https://github.com/spm/spm12/blob/master/toolbox/dcm_meeg/spm_erp_L.m)
  - The lead field (L) is constructed using the specific parameters in P and, where necessary, information in the dipole structure dipfit. For ECD models P.Lpos and P.L encode the position and moments of the ECD. The field `dipfit.type` ('ECD', 'LFP' or 'IMG') determines whether the model is ECD or not. For imaging reconstructions the paramters `P.L` are a (m x n) matrix of coefficients that scale the contrition of n sources to `m = dipfit.Nm` modes encoded in `dipfit.G`. For LFP models (the default), `P.L` simply encodes the electrode gain for each source contributing a LFP.
  
## Special thanks
I want to thank Moritz Gruber for helping me get started on this and sharing his notes and thoughts with me. It made searching through the spm repository a much easier task!
