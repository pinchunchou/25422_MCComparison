
default: RunAll 

Execute: FillHistogram.cpp
	g++ FillHistogram.cpp -o Execute \
		`root-config --libs --cflags`

TestRun: Execute
	./Execute --Input MollyInput/regulate_aa_qcd_extra_weighted_30_F_4.root \
		--Output Output/regulate_aa_qcd_extra_weighted_30_F_4.root \
		--PTHatMin 30 --PTHatMax 50

ExecuteCombine: CombineHistogram.cpp
	g++ CombineHistogram.cpp -o ExecuteCombine \
		`root-config --libs --cflags`

TestRunCombine: ExecuteCombine
	./ExecuteCombine --Input Output/regulate_aa_qcd_extra_weighted_15_F_0.root,Output/regulate_aa_qcd_extra_weighted_30_F_0.root,Output/regulate_aa_qcd_extra_weighted_50_F_0.root,Output/regulate_aa_qcd_extra_weighted_80_F_0.root,Output/regulate_aa_qcd_extra_weighted_120_F_0.root,Output/regulate_aa_qcd_extra_weighted_170_F_0.root \
		--Output regulate_aa_qcd_extra.root

ExecuteShow: ShowHistogram.cpp
	g++ ShowHistogram.cpp -o ExecuteShow \
		`root-config --libs --cflags`

TestRunShow: ExecuteShow
	./ExecuteShow --Input regulate_aa_qcd_extra.root,regulate_aa_qcd_aod.root,regulate_aa_qcd_extra_old.root \
		--Label Extra,AOD,ExtraOld \
		--Output PlotComparison.pdf

RunAll: Execute ExecuteCombine ExecuteShow
	rf -f MollyInput
	ln -sf /data/submit/mitay/photons// MollyInput
	mkdir -p Output
	bash RunAll.sh
