# Default parameter values for prior expectation and prior covariance

`m` represents the number of sources.
`p` is the number of cell populations in each source. Given that we are considering the implementation with the canonical microcircuit: `p=4`.
`u` is the number of inputs.
`Nc` is the number of sensor channels.

I will use common Matlab functions to more concisely express what the parameters are.

A note present in the script [spm_dcm_neural_priors.m](https://github.com/spm/spm12/blob/master/toolbox/dcm_meeg/spm_dcm_neural_priors.m): "*Because priors are specified under log normal assumptions, most parameters are simply scaling coefficients with a prior expectation and variance of one.  After log transform this renders pE = 0 and pC = 1*."

Indeed, check out: [spm_fx_cmm_NMDA.m](https://github.com/spm/spm12/blob/master/toolbox/dcm_meeg/spm_fx_cmm_NMDA.m). There you will see how the prior values you specify are integrated in the model.

## Prior Expectation

| Field  | Meaning | Value  | Questions or comments| Script with documentation about variable / Script where use of variable becomes clear
|:----------|:----------|:----------|:----------|:----------|
| S    | population variance | 0   | | Exponentation here: [spm_fx_cmm_NMDA.m#L153](https://github.com/spm/spm12/blob/master/toolbox/dcm_meeg/spm_fx_cmm_NMDA.m#L153)
| T   |time constants for AMPA, GABA and NMDA channels (3 ion channels). One row for each source.  | `zeros(m,3)`   | | Exponentation here [spm_fx_cmm_NMDA.m#L132-L134](https://github.com/spm/spm12/blob/master/toolbox/dcm_meeg/spm_fx_cmm_NMDA.m#L132-L134)
| G | intrinsic connectivity (unused) |`zeros(m,1)` | Represents the gain and is only applied to the superficial pyramidal cells in each source, hence the dimensions. | Check: [spm_fx_cmm_NMDA.m#L85](https://github.com/spm/spm12/blob/master/toolbox/dcm_meeg/spm_fx_cmm_NMDA.m#L85)
| GN_intrin | input scale to NMDA for each of the m sources. | `zeros(m,1)` | Same parameters for all cells containing NMDA receptor? Or just for pyramidal cells? (see the paramters with "i")|
| GA_intrin | input scale to AMPA for each of the m sources |  `zeros(m,1)` | Idem|
| GG_intrin | input scale to GABA for each of the m sources.| `zeros(m,1)` | Idem |
| GNi_intrin | input scale to interneurons NMDA | `zeros(m,1)` | Now just interneurons? Does it overwrite a previously defined value, like GN_intrin?|
| GAi_intrin | Mg-switch sigmoid scale, slope and sensitivity. Meaning the third entry is unused. | `zeros(m,1)` | Confusing notation, but this parameter refers to NMDA non-linearity. Actually, it is unclear why it has m rows, when only the first three values are used (scale, slope, sensitivity).| spm_fx_cmm_NMDA.m |
| GGi_intrin | input scale to GABA for each of the m sources.| `zeros(m,1)` (unused) | Only for interneurons? | 
| CV | membrane capacitance for all p populations | `zeros(1,p)`| | Exponentation here [spm_fx_cmm_NMDA.m#L145](https://github.com/spm/spm12/blob/master/toolbox/dcm_meeg/spm_fx_cmm_NMDA.m#L145)|
| E | background noise | 0 | | Exponentation here: [spm_fx_cmm_NMDA.m#L166](https://github.com/spm/spm12/blob/master/toolbox/dcm_meeg/spm_fx_cmm_NMDA.m#L166)
| A | extrinsic connections for AMPA (forward {1} and backward {2} connections between sources, row: to, col: from) | `{1×2 cell}`, each cell: `mxm sparse double`, with `-32` if no connection established and `0` if connection established. | How does it know? Where to find this operation? Since the priors are defined in log form, `-32` gets exponentiated and evaluates to essentially `0`. | [spm_cmm_NMDA_priors.m#L82](https://github.com/spm/spm12/blob/master/toolbox/dcm_meeg/spm_cmm_NMDA_priors.m#L82) Exponentiation here: [spm_fx_cmm_NMDA.m#L63](https://github.com/spm/spm12/blob/master/toolbox/dcm_meeg/spm_fx_cmm_NMDA.m#L63)
| AN | extrinsic connections for NMDA (same shape as A)| idem (unused) | idem| Exponentiation here: [spm_fx_cmm_NMDA.m#L65](https://github.com/spm/spm12/blob/master/toolbox/dcm_meeg/spm_fx_cmm_NMDA.m#L65)
| C | subcortical input | `[]` | If dealing with resting-state EEG. | Exponentation here [spm_fx_cmm_NMDA.m#L67](https://github.com/spm/spm12/blob/master/toolbox/dcm_meeg/spm_fx_cmm_NMDA.m#L67)
| H | intrinsic connectivity. | `zeros(p, p, m)`| Unused? Synaptic densities? Rows and columns for one source: **1** - excitatory spiny stellate cells (granular input cells); **2** - superficial pyramidal cells     (forward  output cells); **3** - inhibitory interneurons (intrisic interneuons); **4** - deep pyramidal cells (backward output cells) | Exponentation here: [spm_fx_cmm_NMDA.m#L83](https://github.com/spm/spm12/blob/master/toolbox/dcm_meeg/spm_fx_cmm_NMDA.m#L87)
| R | onset and dispersion |  `zeros(u,2)` --> `0×u empty double matrix` | because `u=0`, in my case. Actual meaning?
| D | delays (unused) | `zeros(m, m)`| Why unused? Why this value?
| Lpos | ROIs (unused) | `sparse(3,0)` | Why unused? When is it used?
| L | leadfield | `zeros(6,m)`| Dimensionality probably due to location (x y z) and direction (x y z).
| J | contributing states |`zeros(1,16)`, except for `(1,2)=1`| What is this?
|a | neuronal innovations (amplitude and exponent) |`zeros(2,m)`| What is this, really? Why these dimensions? | Exponentiation here: [spm_csd_mtf_gu.m#L47](https://github.com/spm/spm12/blob/master/toolbox/dcm_meeg/spm_csd_mtf_gu.m#L47)
|b | channel noise (not source-specific, amplitude and exponent)  |`zeros(2,1)`| 2 channels? | Exponentiation here: [spm_csd_mtf_gu.m#L73](https://github.com/spm/spm12/blob/master/toolbox/dcm_meeg/spm_csd_mtf_gu.m#L73)
|c | channel noise (source-specific, amplitude and exponent) |`zeros(2,1)`| Understand dimensions. This 1 in my model is only one because `size(pE.L,1) ~= 1`, meaning, I had more than one channel (https://github.com/spm/spm12/blob/master/toolbox/dcm_meeg/spm_ssr_priors.m#L42)| Priors set in: [spm_ssr_priors.m#L49](https://github.com/spm/spm12/blob/master/toolbox/dcm_meeg/spm_ssr_priors.m#L49) Exponentation here: [spm_csd_mtf_gu.m#L78](https://github.com/spm/spm12/blob/master/toolbox/dcm_meeg/spm_csd_mtf_gu.m#L78)
|d | neuronal innovations: DCT (discrete cosine transform) basis set coefficients for structured spectra |`zeros(8,m)`| Why is the first dimension set to 8? I thiink this is just standard when using DCT. When using this for images, the image typically gets dividied into 8 by 8 pixel squares and you calculate the basis functions for each of these squares. For a great introduction of DCT, see [here](https://www.youtube.com/watch?v=Q2aEzeMDHMA). In our case, we are dealing with a one-dimensional signal, so what is happening here is that you are calculating the 8 coefficients needed to represent the signal from each source. | Exponentation here: [spm_csd_mtf_gu.m#L58](https://github.com/spm/spm12/blob/master/toolbox/dcm_meeg/spm_csd_mtf_gu.m#L58)



## Prior covariance

We now turn to the covariance parameter of these. Since the the meaning of the parameters stays the same, we've left out the 'meaning' column.

| Field  | Value  | Questions or comments| Script with documentation about variable
|:----------|:----------|:----------|:----------|
| S    		| 1/64 | Why this particular value? How was it found? | Defined in [spm_cmm_NMDA_priors.m](https://github.com/spm/spm12/blob/master/toolbox/dcm_meeg/spm_cmm_NMDA_priors.m) and script includes citation of David O, Friston KJ (2003) "A neural mass model for MEG/EEG: coupling and neuronal dynamics". *NeuroImage* 20: 1743-1755.
| T | `ones(m,3)/16` | Idem
| G | `zeros(m,	1)/16` (unused)| Zero expectation and zero variance?? Where is within source connectivity defined?|  Defined in [spm_cmm_NMDA_priors.m](https://github.com/spm/spm12/blob/master/toolbox/dcm_meeg/spm_cmm_NMDA_priors.m)
| GN_intrin | `ones(m,1)/8`| Why this value?|
| GA_intrin | `ones(m,1)/8`| Idem|
| GG_intrin | `ones(m,1)/8`| Idem|
| GNi_intrin | `ones(m,1)/8`| Idem|
| GAi_intrin |`ones(m,1)/8`| Idem|
| GGi_intrin | `ones(m,1)/8` (unused) | Idem|
| CV | `zeros(1,p)/16` | Zero expectation and zero variance?? How can I always have membrane capacitance?|
| E | 1/128 | Why this value?|
| A | `{1×2 cell}`, each cell: `mxm double`, with `0` if no connection established and `1/8` if connection established. | So `0` covariance for the expected value `-32`? Again, why this value? And why fix it? | [spm_cmm_NMDA_priors.m#L83](https://github.com/spm/spm12/blob/master/toolbox/dcm_meeg/spm_cmm_NMDA_priors.m#L83)
| AN | Idem (unused)| Idem
| C | `[]`
| H | `zeros(p, p, m)` | Rows and columns for one source: 1 - excitatory spiny stellate cells (granular input cells); 2 - superficial pyramidal cells     (forward  output cells); 3 - inhibitory interneurons (intrisic interneuons); 4 - deep pyramidal cells (backward output cells)
| R | `ones(u,1)*[1/16 1/16]` --> `0×u empty double matrix` because `u=0`
| D | `speye(m,m)/32` (unused) | Why this value?
| Lpos | `sparse(3,0)` (unused) 
| L | `64*ones(Nc,m)` | Why 64?
| J | `zeros(1,16)`, expect for `(1,3)=1/32` and `(1,4)=1/32`| Why this value?
|a | `sparse(2,m) + 1/128` | Why?
|b | `sparse(2,1) + 1/128` | Why?
|c | Idem | Idem
|d |`sparse(8,m) + 1/128`| Why this particular value? Why this first dimension `d`. | `d` defined as `8` in the version of SPM I am using, but as `4` in [spm_ssr_priors.m#L53](https://github.com/spm/spm12/blob/master/toolbox/dcm_meeg/spm_ssr_priors.m#L53). Why this discrepancy??
