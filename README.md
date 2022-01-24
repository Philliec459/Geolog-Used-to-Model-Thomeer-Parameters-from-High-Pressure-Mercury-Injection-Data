# Geolog-Used-to-Model-Thomeer-Parameters-from-High-Pressure-Mercury-Injection-Data
This repository contains a complete compressed (zipped) Geolog Project that has been used to model SCAL High Pressure Mercury Injection (HPMI) core data using Thomeer Capillary Pressure parameters. 
---
### Updated January 24, 2022 with python 'try' implementation and calculations of error, Thomeer perm and Mode of Pore Thoat Distribution in Microns (diameter). There are now a number of samples that can be used. We have added an option to create *.png image from the Final Plot too. 

### Right now our Thomeer Workflow has all of the Closure Corrections by sample as a constant called Closure. We think it is best to use a series of cross plots in Geolog or python matplotlib to pick this Closure Correction from the HPMI data up front before starting on the Thomeer Analysis of the HPMI data. The Closure Correction is given in the same line as the selection of the Sample file that you select. We also have a curve called NO_PORE_SYS that is used to instruct the Notebook as to how many pore systems to solve for in this process. 

### In this Jupyter Notebook we also have an Automatic option (‘YES’,’NO’) that if Auto=‘NO’, then the python loglan will allow you to make your own picks for the following:

      1) Pd1
      2) BV1 & Pd2 if 2 Pore Systems
      3) BVtotal if 2 Pore Systems

### If Auto = 'YES' and your sample has 2 pore systems, then you would still need to pick Pd2 and BV1 from item #2 above.However, please still consider all of this work in progress. 
---
## Introduction:
This GitHub repository uses python loglan code to load High Pressure Mercury Injection (HPMI) Core data directly from the SCAL set in a Geolog Well and then model the HPMI data using a Thomeer hyperbola by our estimations of the Thomeer Capillary Pressure parameters as shown below. Ed Clerke used a similar method in Excel with Solver to estimate his Thomeer parameters for each HPMI sample that went into the Rosetta Stone Arab D Carbonate Thomeer database. We have also used fminsearch in Matlab too, but with this repository we are able to use Geolog to perform the modeling of the HPMI data. The purpose of this Geolog project is to serve as an example that will allow the user to build their own reservoir-specific core calibration database for their Reservoir Characterization studies. 

![HPMI_Image](HPMI.png)

## How it Works:
The following animated image illustrates the process in Geolog. We start with the original HPMI data. The first step is to locate the point on the HPMI curve that represents the point where real matrix data begins and not the HPMI data representing surface conformance around the plug sample. We now believe that the best way to pick the Closure Correction is by using the the Log-Log and Linear-log BVocc vs. Pc cross plots while in Geolog and inputting this Closure Correction as a curve called Closure in the SCAL set for each sample first prior to the Thomeer Analysis. 

![HPMI_Image](PickClosure.png)

For the second step we pick the point for the Bulk Volume porosity of the first pore system (BV1) as well as the Initial Displacement Pressure for the second pore system (Pd2) if available from the sample data.

The third step is to select the Total porosity for the HPMI data called BVtotal where the porosity of the second pore system is called BV2:

      BV2 = BVtotal - BV1

![HPMI_Image](Thomeer_Parameter_fitting_Geolog2.gif)

This program uses Scipy Optimize curve_fit to estimate the appropriate Thomeer parameters necessary to model the HPMI data. The points selected from the GUIs are used to estimate the boundary conditions for these estimations, and the estimations for a typical example are shown below where we have two pore systems available in the sample. 

    **Thomeer Parameters Estimated from Imported HPMI Data:**
        Pd1 = 8.67  ,  G1 = 0.54 , BV1 = 10.13
        Pd2 = 389.1 ,  G2 = 0.24 , BV2 = 4.8


## Reservoir Characterization Workflow:
We now have numerous HPMI samples in this Geolog project in the SCAL set. In Geolog we first read-in the Capillary Pressure (Pc) data directly from the Well, perform our Thomeer analysis on each sample and then write the results back to Geolog. We use this process to build our own sample-by-sample core calibration database. After all the Thomeer analysis would be performed, then we would then use these results in a Reservoir Characterization workflow similar to what we employed in the following GitHub repository. Of course, we would alter the workflow to employ our own new reservoir-specific calibration data for our Reservoir Characterization studies.

https://github.com/Philliec459/Geolog-Used-to-Automate-the-Characterization-Workflow-using-Clerkes-Rosetta-Stone-calibration-data

In the above workflow example, we used hundreds of HPMI samples as calibration data. In the image below we are showing a Porosity vs. Permeability cross plot in the upper left for all the calibration samples used in Ed Clerke's Rosetta Stone calibration data for the Arab D reservoir. The colors represent different Petrophysical Rock Type (PRT) as defined by Clerke. In this example we select a small group of poro-perm samples, and then the Pc curves from this small group of selected samples is then shown in the lower left. The black Pc curve is the upscaled Pc curve from the selected samples, and the black bars in the histograms represent the median value for the selected samples. We would be using the upscaled Pc curves that vary level-by-level in the well to model saturations over a vast range of reservoir quality rock along the wellbore.

![HPMI_Image](Thomeer_Pc_and_Thomeer_Parameters2.gif)

## Modeling of Saturations Using Thomeer Capillary Pressure Parameters: 
Also, the following image shows one example from our modeling of saturations from Capillary Pressure vs. log analysis. The match is very good. I personally have performed this type of characterization on at least 30 huge carbonate oil fields in Saudi, and the results shown below are very typical.

![HPMI_Image](logsats.gif)

---
1 Clerke, E. A., Mueller III, H. W., Phillips, E. C., Eyvazzadeh, R. Y., Jones, D. H., Ramamoorthy, R., Srivastava, A., (2008) “Application of Thomeer Hyperbolas to decode the pore systems, facies and reservoir properties of the Upper Jurassic Arab D Limestone, Ghawar field, Saudi Arabia: A Rosetta Stone approach”, GeoArabia, Vol. 13, No. 4, p. 113-160, October 2008.
