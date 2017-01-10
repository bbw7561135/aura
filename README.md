#Aura

Welcome to Aura (I don't have a cool acronym yet, I just like the name). Formerly this code in a previous iteration was called Cosmicp but this is a way better name.  

##OVERVIEW

Aura takes any arbitrary cosmic ray (CR) spectrum and along with information about the ambient medium generates radiative byproducts for a variety of interactions.  Currently included in Aura are:

-Synchrotron-

-Inverse Compton (IC)-

-Bremsstrahlung-

-Proton-Proton Interactions-

Aura is intended to be an updated version of the Cosmicp code (see Edmon et.al. (2011) for details).  It is written completely in FORTRAN 90 using a pseudo-object oriented style.

##USAGE

The usage for Aura is straight forward.  A user only needs to edit 2 files.  The first is the AURA.data namelist.  The namelist is divided into two portions, one regarding the inputs called the Information namelist, the other deals with the outputs called the Emission namelist.  A description of the components required for each namelist can be found in Aura.data.dist.

The second file is the mod_user.f90 file.  Within the user will find two subroutines labelled User_Env and User_CR.  User_Env allows one to define the environment in question.  All the environment variables are listed in the example file as well as the units that the variables should be in.  The environment variables actually used by the program vary depending on which type of radiation one is simulating.  Here is a list of what is required for each radiation type:

Synchrotron: n_e (electron number density), B (magnetic field strength), theta (magnetic field angle with respect to the observer)

IC: nph, nuph (ambient photon spectrum and frequency binning)

Bremsstrahlung: n_e, n_p (proton number density), n_alpha (alpha particle density), n_H (neutral Hydrogen density), n_He (neutral Helium density)

Proton-Proton Interactions: n_p, n_H

The units for these ambient variables are CGS with ambient photon spectrum for IC (assumed to be isotropic) being in [photons/cm^3/erg].

The User_CR routine defines the CR spectrum and species. One needs to define the charge (q) and mass (m) of the CR.  Then one defines the kinetic energy range for the spectrum (e), finally one defines the spectrum itself (n) which is in units of [particles/cm^3/erg] and is assumed to be dN/dE where E in this case is the kinetic energy.

Once you are done then just compile and run.  That's it.  Aura runs on a single zone model, so for the given conditions it will generate emissivities (j_nu) and particle source functions (q_particle type).  The emissivities are in [ergs/cm^3/s/Hz/str], where as the source functions are in [particles/cm^3/s/erg].  These files are output to the directory defined in AURA.data along with the defined header.  If one wishes to edit the output units, simply look at mod_io.f90.  The specific routines that calculate the emission output emissivities and source functions so it should be easy to convert to what ever units on desires.

##CAVEATS

This is a single zone model so it currently does not handle large blocks of data.  That being said the code can be modified fairly easily to iterate over large blocks of data if that is desired.

The solutions from Aura are good to within 1% if the CR spectrum and ambient photon spectrum have ~10 bins logarithmically spaced per decade.  Changing the number of output bins does not impact the accuracy of the calculation as each bin is calculated independantly.  If one sees oscillations in the spectrum, your CR spectral resolution or ambient photon field is too coarse for the output emission resolution.  Cranking up the resolution in the CR spectrum will resolve this issue.  The oscillations are due to insufficient sampling of the spectrum.  This causes the spectrum to look like the sum of multiple single particle emission spectra all at different energies rather than a smooth spectrum.  Simple tests can show what resolution will work for the problem you have in mind.

Secondaries are currently simply output from the code.  However it is easy to feed the secondary electrons and positrons back into the code to calculate their emissions.

The code currently handles a single CR species.  Multiple CR species would be an extension which could be done.

Synchrotron and Bremsstrahlung can calculate emissions from electrons, positrons, protons and nuclei.  IC can only calculates emissions for electrons and positrons, while Proton-Proton collisions only works for protons.

##PAPERS

Other than the papers referenced in the code itself you will want to see this paper here for an overview:

http://adsabs.harvard.edu/abs/2011MNRAS.414.3521E

Other examples of the use of this code can be found here:

http://adsabs.harvard.edu/abs/2012ApJ...745..146K

http://adsabs.harvard.edu/abs/2013ApJ...777...25K

##PROBLEMS? CONCERNS? COMMENTS? DEEP THOUGHTS?

Contact Paul P. Edmon (pedmon@cfa.harvard.edu)

##REFERENCES

It is asked that those who use this code refernce the original paper in MNRAS.
