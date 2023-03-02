#!/bin/bash

Fraction=0.01

for i in 15_30 30_50 50_80 80_120 120_170 170_2000
do
   PTHatMin=`echo $i | cut -d '_' -f 1`
   PTHatMax=`echo $i | cut -d '_' -f 2`
   
   echo Running PTHatMin = $PTHatMin...
	./Execute --Input MollyInput/regulate_aa_qcd_extra_weighted_${PTHatMin}_F_0.root --Fraction $Fraction \
		--Output Output/regulate_aa_qcd_extra_weighted_${PTHatMin}_F_0.root \
		--PTHatMin $PTHatMin --PTHatMax $PTHatMax \
      --PVz 0.032223,0.433841,4.954461,0.032146,0.909113,4.964358
	
   ./Execute --Input MollyInput/regulate_aa_qcd_aod_weighted_${PTHatMin}_F.root --Fraction $Fraction \
		--Output Output/regulate_aa_qcd_aod_weighted_${PTHatMin}_F_0.root \
		--PTHatMin $PTHatMin --PTHatMax $PTHatMax \
      --PVz 0.032223,0.433841,4.954461,0.032138,0.910570,4.965655
	
   ./Execute --Input MollyInput/regulate_aa_qcd_extra_old_weighted_${PTHatMin}_F.root --Fraction $Fraction \
		--Output Output/regulate_aa_qcd_extra_old_weighted_${PTHatMin}_F_0.root \
		--PTHatMin $PTHatMin --PTHatMax $PTHatMax \
      --PVz 0.032223,0.433841,4.954461,0.032147,0.909282,4.964185
done

./ExecuteCombine --Input Output/regulate_aa_qcd_extra_weighted_15_F_0.root,Output/regulate_aa_qcd_extra_weighted_30_F_0.root,Output/regulate_aa_qcd_extra_weighted_50_F_0.root,Output/regulate_aa_qcd_extra_weighted_80_F_0.root,Output/regulate_aa_qcd_extra_weighted_120_F_0.root,Output/regulate_aa_qcd_extra_weighted_170_F_0.root \
		--Output regulate_aa_qcd_extra.root

./ExecuteCombine --Input Output/regulate_aa_qcd_aod_weighted_15_F_0.root,Output/regulate_aa_qcd_aod_weighted_30_F_0.root,Output/regulate_aa_qcd_aod_weighted_50_F_0.root,Output/regulate_aa_qcd_aod_weighted_80_F_0.root,Output/regulate_aa_qcd_aod_weighted_120_F_0.root,Output/regulate_aa_qcd_aod_weighted_170_F_0.root \
		--Output regulate_aa_qcd_aod.root

./ExecuteCombine --Input Output/regulate_aa_qcd_extra_old_weighted_15_F_0.root,Output/regulate_aa_qcd_extra_old_weighted_30_F_0.root,Output/regulate_aa_qcd_extra_old_weighted_50_F_0.root,Output/regulate_aa_qcd_extra_old_weighted_80_F_0.root,Output/regulate_aa_qcd_extra_old_weighted_120_F_0.root,Output/regulate_aa_qcd_extra_old_weighted_170_F_0.root \
		--Output regulate_aa_qcd_extra_old.root
	
./ExecuteShow --Input regulate_aa_qcd_extra.root,regulate_aa_qcd_aod.root,regulate_aa_qcd_extra_old.root \
	--Label Extra,AOD,ExtraOld \
	--Output PlotComparison.pdf

