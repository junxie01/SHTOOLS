---
title: SHBiasKMask (Fortran)
keywords: spherical harmonics software package, spherical harmonic transform, legendre functions, multitaper spectral analysis, fortran, Python, gravity, magnetic field
sidebar: fortran_sidebar
permalink: shbiaskmask.html
summary:
tags: [fortran]
toc: false
editdoc: fdoc
---

Calculate the multitaper (cross-)power spectrum expectation of a function localized by arbitrary windows derived from a mask.

## Usage

call SHBiasK (`tapers`, `lwin`, `k`, `incspectra`, `ldata`, `outcspectra`, `taper_wt`, `save_cg`, `exitstatus`)

## Parameters

`tapers` : input, real(dp), dimension ((`lwin`+1)**2, `k`)
:   The spherical harmonic coefficients of the localization windows generated by a call to `SHReturnTapersMap`. The coefficients in each column are ordered according to the convention in `SHCilmToVector`.

`lwin` : input, integer
:   The spherical harmonic bandwidth of the localizing windows.

`k` : input, integer
:   The number of localization windows to use. Only the first `k` columns of `tapers` will be employed, which corresponds to the best-concentrated windows.

`incspectra` : input, real(dp), dimension (`ldata`+1)
:   The global unwindowed power spectrum.

`ldata` : input, integer
:   The maximum degree of the global unwindowed power spectrum.

`outcspectra` : output, real(dp), dimension (`ldata`+`lwin`+1)
:   The expectation of the localized multitaper power spectrum.

`taper_wt` : input, optional, real(dp), dimension (`k`)
:   The weights to apply to each individual windowed spectral estimate.

`save_cg` : input, optional, integer, default = 0
:   If set equal to 1, the Clebsch-Gordon coefficients will be precomputed and saved for future use (if `lwin` or `ldata` change, these will be recomputed). To deallocate the saved memory, set this parameter equal to 1. If set equal to 0 (default), the Clebsch-Gordon coefficients will be recomputed for each call.

`exitstatus` : output, optional, integer
:   If present, instead of executing a STOP when an error is encountered, the variable exitstatus will be returned describing the error. 0 = No errors; 1 = Improper dimensions of input array; 2 = Improper bounds for input variable; 3 = Error allocating memory; 4 = File IO error.

## Description

`SHBiasKMask` will calculate the multitaper (cross-)power spectrum expectation of a function multiplied by the `k` best-concentrated localization windows derived from an arbitrary mask. This is given by equation 36 of Wieczorek and Simons (2005) (see also eq. 2.11 of Wieczorek and Simons 2007). In contrast to `SHBias`, which takes as input the power spectrum of a single localization window, this routine expects as input a matrix containing the spherical harmonic coefficients of the windows. These can be generated by a call to `SHReturnTapersMap` and the coefficients in each column are ordered according to the convention in `SHCilmToVector`.

The maximum calculated degree of the windowed power spectrum expectation corresponds to the smaller of (`ldata`+`lwin`) and `size(outcspectra)-1`. It is assumed implicitly that the power spectrum of `inspectrum` is zero beyond degree `ldata`. If this is not the case, the ouput power spectrum should be considered valid only for the degrees up to and including `ldata` - `lwin`.

The default is to apply equal weights to each individual windowed estimate of the spectrum, but this can be modified by specifying the weights in the optional argument `taper_wt`. The weights must sum to unity. If this routine is to be called several times using the same values of `lwin` and `ldata`, then the Clebsch-Gordon coefficients can be precomputed and saved by setting the optional parameter `save_cg` equal to 1.

## References

Wieczorek, M. A. and F. J. Simons, Minimum-variance multitaper spectral estimation on the sphere, J. Fourier Anal. Appl., 13, 665-692, doi:10.1007/s00041-006-6904-1, 2007.

Simons, F. J., F. A. Dahlen and M. A. Wieczorek, Spatiospectral concentration on a sphere, SIAM Review, 48, 504-536, doi:10.1137/S0036144504445765, 2006. 

Wieczorek, M. A. and F. J. Simons, Localized spectral analysis on the sphere, 
Geophys. J. Int., 162, 655-675, doi:10.1111/j.1365-246X.2005.02687.x, 2005.

## See also

[shbias](shbias.html), [shreturntapersmap](shreturntapersmap.html), [shmtcouplingmatrix](shmtcouplingmatrix.html)
