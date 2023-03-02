#include <iostream>
using namespace std;

#include "TFile.h"
#include "TTree.h"
#include "TH1D.h"
#include "TF1.h"

#include "ProgressBar.h"
#include "CommandLine.h"

int main(int argc, char *argv[]);
double GetWeight(double vz, vector<double> &p);

int main(int argc, char *argv[])
{
   CommandLine CL(argc, argv);

   string InputFileName = CL.Get("Input");
   string OutputFileName = CL.Get("Output");
   double Fraction = CL.GetDouble("Fraction", 1.00);
   double PTHatMin = CL.GetDouble("PTHatMin");
   double PTHatMax = CL.GetDouble("PTHatMax");

   vector<double> PVz = CL.GetDoubleVector("PVz", vector<double>{0.032223, 0.433841, 4.954461, 0.032146, 0.909113, 4.964358});

   TFile FInput(InputFileName.c_str());
   TFile FOutput(OutputFileName.c_str(), "RECREATE");

   TH1D HPTHatMin("HPTHatMin", "", 1000, 0, 2000);
   TH1D HPTHatMax("HPTHatMax", "", 1000, 0, 2000);
   TH1D HPTHat("HPTHat", "", 100, 0, 1000);
   TH1D HVzNoWeight("HVzNoWeight", "", 100, -20, 20);
   TH1D HVz("HVz", "", 100, -20, 20);

   TH1D HNGenJet("HNGenJet", "|eta| < 1.6", 25, 0, 25);
   TH1D HNGenJet30("HNGenJet30", "|eta| < 1.6, PT > 30", 14, 0, 14);
   TH1D HNGenJet40("HNGenJet40", "|eta| < 1.6, PT > 40", 14, 0, 14);
   TH1D HGenJetPT("HGenJetPT", "|eta| < 1.6", 100, 0, 500);
   TH1D HGenJetEta("HGenJetEta", "PT > 30", 100, -5, 5);
   TH1D HGenJetPhi("HGenJetPhi", "|eta| < 1.6, PT > 30", 100, -M_PI, M_PI);
   
   TH1D HNJet("HNJet", "|eta| < 1.6", 50, 0, 50);
   TH1D HNJet30("HNJet30", "|eta| < 1.6, PT > 30", 15, 0, 15);
   TH1D HNJet40("HNJet40", "|eta| < 1.6, PT > 40", 15, 0, 15);
   TH1D HRefJetPT("HRefJetPT", "|eta| < 1.6", 100, 0, 500);
   TH1D HRefJetEta("HRefJetEta", "PT > 30", 100, -5, 5);
   TH1D HRefJetPhi("HRefJetPhi", "|eta| < 1.6, PT > 30", 100, -M_PI, M_PI);
   TH1D HJetPT("HJetPT", "|eta| < 1.6", 100, 0, 500);
   TH1D HJetEta("HJetEta", "PT > 30", 100, -5, 5);
   TH1D HJetPhi("HJetPhi", "|eta| < 1.6, PT > 30", 100, -M_PI, M_PI);
   TH1D HJetWTAEta("HJetWTAEta", "PT > 30", 100, -5, 5);
   TH1D HJetWTAPhi("HJetWTAPhi", "|eta| < 1.6, PT > 30", 100, -M_PI, M_PI);
   TH1D HJetDEta("HJetDEta", "|eta| < 1.6, PT > 30", 100, -0.5, 0.5);
   TH1D HJetDPhi("HJetDPhi", "|eta| < 1.6, PT > 30", 100, -0.5, 0.5);
   TH1D HJetDR("HJetDR", "|eta| < 1.6, PT > 30", 100, 0, 1);

   TH1D HNPho("HNPho", "|eta| < 1.442", 14, 0, 14);
   TH1D HNPho40("HNPho40", "|eta| < 1.442, Et > 40", 10, 0, 10);
   TH1D HNPho60("HNPho60", "|eta| < 1.442, Et > 60", 10, 0, 10);
   TH1D HPhoEta("HPhoEta", "Et > 40", 100, -2.5, 2.5);
   TH1D HPhoEt("HPhoEt", "|eta| < 1.442", 100, 0, 250);
   TH1D HPhoPhi("HPhoPhi", "|eta| < 1.442, Et > 40", 100, -M_PI, M_PI);
   TH1D HPhoHoverE("HPhoHoverE", "|eta| < 1.442, Et > 40", 100, 0, 1);

   TH1D HLeadingPhoFound("HLeadingPhoFound", "Found?", 2, 0, 2);
   TH1D HLeadingPhoEta("HLeadingPhoEta", "Et > 40", 100, -2.5, 2.5);
   TH1D HLeadingPhoEt("HLeadingPhoEt", "|eta| < 1.442", 100, 0, 250);
   TH1D HLeadingPhoPhi("HLeadingPhoPhi", "|eta| < 1.442, Et > 40", 100, -M_PI, M_PI);
   TH1D HLeadingPhoSigmaIEtaIEta("HLeadingPhoSigmaIEtaIEta", "|eta| < 1.442, Et > 40", 100, 0, 0.05);
   TH1D HLeadingPhoEcalIso("HLeadingPhoEcalIso", "|eta| < 1.442, Et > 40", 100, 0, 10);
   TH1D HLeadingPhoHcalIso("HLeadingPhoHcalIso", "|eta| < 1.442, Et > 40", 100, 0, 10);
   TH1D HLeadingPhoTrackIso("HLeadingPhoTrackIso", "|eta| < 1.442, Et > 40", 100, 0, 10);
   TH1D HLeadingPhoIso("HLeadingPhoIso", "|eta| < 1.442, Et > 40", 100, 0, 10);
   TH1D HLeadingPhoSelected("HLeadingPhoSelected", "Selected?", 2, 0, 2);

   TH1D HSelectedPhoEta("HSelectedPhoEta", "Et > 40", 100, -2.5, 2.5);
   TH1D HSelectedPhoEt("HSelectedPhoEt", "|eta| < 1.442", 100, 0, 250);
   TH1D HSelectedPhoPhi("HSelectedPhoPhi", "|eta| < 1.442, Et > 40", 100, -M_PI, M_PI);

   TH1D HPhotonJetDPhi("HPhotonJetDPhi", "", 100, -M_PI, M_PI);
   TH1D HPhotonJetWTADPhi("HPhotonJetWTADPhi", "", 100, -M_PI, M_PI);

   TH1D HSelectedJetPT("HSelectedJetPT", "", 100, 0, 250);
   TH1D HSelectedJetEta("HSelectedJetEta", "", 100, -2, 2);
   TH1D HSelectedJetPhi("HSelectedJetPhi", "", 100, -M_PI, M_PI);
   TH1D HSelectedJetDEtaWide("HSelectedJetDEtaWide", "", 100, -2.5, 2.5);
   TH1D HSelectedJetDEta("HSelectedJetDEta", "", 100, -0.3, 0.3);
   TH1D HSelectedJetDPhiWide("HSelectedJetDPhiWide", "", 100, -M_PI, M_PI);
   TH1D HSelectedJetDPhi("HSelectedJetDPhi", "", 100, -0.3, 0.3);
   TH1D HSelectedJetDR("HSelectedJetDR", "", 100, 0, 0.3);

   TTree *Tree = (TTree *)FInput.Get("pj");

   float pthat;
   float weight;
   float w;
   float vz;
   Tree->SetBranchAddress("pthat", &pthat);
   Tree->SetBranchAddress("weight", &weight);
   Tree->SetBranchAddress("w", &w);
   Tree->SetBranchAddress("vz", &vz);

   int ngen, nref;
   vector<float> *genpt = nullptr;
   vector<float> *geneta = nullptr;
   vector<float> *genphi = nullptr;
   vector<float> *refpt = nullptr;
   vector<float> *refeta = nullptr;
   vector<float> *refphi = nullptr;
   vector<float> *jtpt = nullptr;
   vector<float> *jteta = nullptr;
   vector<float> *jtphi = nullptr;
   vector<float> *WTAeta = nullptr;
   vector<float> *WTAphi = nullptr;
   Tree->SetBranchAddress("ngen", &ngen);
   Tree->SetBranchAddress("genpt", &genpt);
   Tree->SetBranchAddress("geneta", &geneta);
   Tree->SetBranchAddress("genphi", &genphi);
   Tree->SetBranchAddress("nref", &nref);
   Tree->SetBranchAddress("refpt", &refpt);
   Tree->SetBranchAddress("refeta", &refeta);
   Tree->SetBranchAddress("refphi", &refphi);
   Tree->SetBranchAddress("jtptCor", &jtpt);
   Tree->SetBranchAddress("jteta", &jteta);
   Tree->SetBranchAddress("jtphi", &jtphi);
   Tree->SetBranchAddress("WTAeta", &WTAeta);
   Tree->SetBranchAddress("WTAphi", &WTAphi);

   int npho;
   vector<float> *phoEt = nullptr;
   vector<float> *phoRawEt = nullptr;
   vector<float> *phoEta = nullptr;
   vector<float> *phoPhi = nullptr;
   vector<float> *phoHoverE = nullptr;
   vector<float> *phoSigmaIEtaIEta_2012 = nullptr;
   vector<float> *pho_ecalClusterIsoR3 = nullptr;
   vector<float> *pho_hcalRechitIsoR3 = nullptr;
   vector<float> *pho_trackIsoR3PtCut20 = nullptr;
   Tree->SetBranchAddress("nPho", &npho);
   Tree->SetBranchAddress("phoEtErNew", &phoEt);
   Tree->SetBranchAddress("phoEt", &phoRawEt);
   Tree->SetBranchAddress("phoSCEta", &phoEta);
   Tree->SetBranchAddress("phoSCPhi", &phoPhi);
   Tree->SetBranchAddress("phoHoverE", &phoHoverE);
   Tree->SetBranchAddress("phoSigmaIEtaIEta_2012", &phoSigmaIEtaIEta_2012);
   Tree->SetBranchAddress("pho_ecalClusterIsoR3", &pho_ecalClusterIsoR3);
   Tree->SetBranchAddress("pho_hcalRechitIsoR3", &pho_hcalRechitIsoR3);
   Tree->SetBranchAddress("pho_trackIsoR3PtCut20", &pho_trackIsoR3PtCut20);

   int EntryCount = Tree->GetEntries() * Fraction;
   ProgressBar Bar(cout, EntryCount);
   Bar.SetStyle(-1);
   for(int iE = 0; iE < EntryCount; iE++)
   {
      if(EntryCount < 200 || (iE % (EntryCount / 200) == 0))
      {
         Bar.Update(iE);
         Bar.Print();
      }

      Tree->GetEntry(iE);

      double wz = GetWeight(vz, PVz);

      if(pthat >= PTHatMin)
         HPTHatMin.Fill(pthat, wz);
      if(pthat >= PTHatMax)
         HPTHatMax.Fill(pthat, wz);
      if(pthat >= PTHatMin && pthat < PTHatMax)
         HPTHat.Fill(pthat, wz);

      if(pthat < PTHatMin || pthat >= PTHatMax)
         continue;

      HVzNoWeight.Fill(vz, 1);
      HVz.Fill(vz, wz);

      int ngen0 = 0, ngen30 = 0, ngen40 = 0;
      for(int iJ = 0; iJ < ngen; iJ++)
      {
         if((*genpt)[iJ] > 30)
            HGenJetEta.Fill((*geneta)[iJ], wz);
        
         if((*geneta)[iJ] > 1.6 || (*geneta)[iJ] < -1.6)
            continue;

         HGenJetPT.Fill((*genpt)[iJ], wz);
         if((*genpt)[iJ] > 30)
            HGenJetPhi.Fill((*genphi)[iJ], wz);

         if((*genpt)[iJ] > 0)    ngen0 = ngen0 + 1;
         if((*genpt)[iJ] > 30)   ngen30 = ngen30 + 1;
         if((*genpt)[iJ] > 40)   ngen40 = ngen40 + 1;
      }
      HNGenJet.Fill(ngen0, wz);
      HNGenJet30.Fill(ngen30, wz);
      HNGenJet40.Fill(ngen40, wz);
     
      int njet0 = 0, njet30 = 0, njet40 = 0;
      for(int iJ = 0; iJ < nref; iJ++)
      {
         if((*jteta)[iJ] < -1.3 && (*jtphi)[iJ] < -0.8 && (*jtphi)[iJ] > -1.7)
            continue;
         if((*jteta)[iJ] < 1.5 && (*jteta)[iJ] > -0.5 && (*jtphi)[iJ] < 1.0 && (*jtphi)[iJ] > 0)
            continue;
         if((*jteta)[iJ] < 1.6 && (*jteta)[iJ] > 1.5 && (*jtphi)[iJ] < -1.0 && (*jtphi)[iJ] > -1.5)
            continue;

         if((*refpt)[iJ] > 30)
            HRefJetEta.Fill((*refeta)[iJ], wz);
         if((*jtpt)[iJ] > 30)
            HJetEta.Fill((*jteta)[iJ], wz);
         if((*jtpt)[iJ] > 30)
            HJetWTAEta.Fill((*WTAeta)[iJ], wz);
         
         if((*jteta)[iJ] > 1.6 || (*jteta)[iJ] < -1.6)
            continue;
         
         HRefJetPT.Fill((*refpt)[iJ], wz);
         if((*refpt)[iJ] > 30)
            HRefJetPhi.Fill((*refphi)[iJ], wz);
         HJetPT.Fill((*jtpt)[iJ], wz);
         if((*jtpt)[iJ] > 30)
            HJetPhi.Fill((*jtphi)[iJ], wz);
         
         if((*jtpt)[iJ] > 30)
            HJetWTAPhi.Fill((*WTAphi)[iJ], wz);

         if((*jtpt)[iJ] > 0)    njet0 = njet0 + 1;
         if((*jtpt)[iJ] > 30)   njet30 = njet30 + 1;
         if((*jtpt)[iJ] > 40)   njet40 = njet40 + 1;

         double DEta = (*jteta)[iJ] - (*WTAeta)[iJ];
         double DPhi = (*jtphi)[iJ] - (*WTAphi)[iJ];
         if(DPhi > +M_PI)   DPhi = DPhi - 2 * M_PI;
         if(DPhi < -M_PI)   DPhi = DPhi + 2 * M_PI;
         double DR = sqrt(DEta * DEta + DPhi * DPhi);

         HJetDEta.Fill(DEta, wz);
         HJetDPhi.Fill(DPhi, wz);
         HJetDR.Fill(DR, wz);
      }
      HNJet.Fill(njet0, wz);
      HNJet30.Fill(njet30, wz);
      HNJet40.Fill(njet40, wz);

      int lP = -1;   // leading photon index
      int npho0 = 0, npho40 = 0, npho60 = 0;
      for(int iP = 0; iP < npho; iP++)
      {
         if((*phoRawEt)[iP] < 30)   // otherwise we see weirdness
            continue;

         if((*phoEta)[iP] < -1.3 && (*phoPhi)[iP] < -0.7 && (*phoPhi)[iP] > -1.6)
            continue;
         if((*phoEta)[iP] < 1.5 && (*phoEta)[iP] > 0.1 && (*phoPhi)[iP] < 1.0 && (*phoPhi)[iP] > 0.2)
            continue;

         if((*phoEt)[iP] > 40)
            HPhoEta.Fill((*phoEta)[iP], wz);

         if((*phoEta)[iP] < -1.442 || (*phoEta)[iP] > 1.442)
            continue;

         if((*phoEt)[iP] > 40)
            HPhoHoverE.Fill((*phoHoverE)[iP], wz);

         HPhoEt.Fill((*phoEt)[iP], wz);
         if((*phoEt)[iP] > 40)
            HPhoPhi.Fill((*phoPhi)[iP], wz);

         if((*phoEt)[iP] > 0)    npho0 = npho0 + 1;
         if((*phoEt)[iP] > 40)   npho40 = npho40 + 1;
         if((*phoEt)[iP] > 60)   npho60 = npho60 + 1;

         if(lP < 0 || (*phoEt)[lP] < (*phoEt)[iP])
            lP = iP;
      }
      HNPho.Fill(npho0, wz);
      HNPho40.Fill(npho40, wz);
      HNPho60.Fill(npho60, wz);

      bool Selected = false;
      HLeadingPhoFound.Fill(lP >= 0, wz);
      if(lP >= 0)
      {
         HLeadingPhoEt.Fill((*phoEt)[lP], wz);
         if((*phoEt)[lP] > 40)
            HLeadingPhoEta.Fill((*phoEta)[lP], wz);
         if((*phoEt)[lP] > 40)
            HLeadingPhoPhi.Fill((*phoPhi)[lP], wz);

         double SEE = (*phoSigmaIEtaIEta_2012)[lP];
         double EcalIso = (*pho_ecalClusterIsoR3)[lP];
         double HcalIso = (*pho_hcalRechitIsoR3)[lP];
         double TrackIso = (*pho_trackIsoR3PtCut20)[lP];

         HLeadingPhoSigmaIEtaIEta.Fill(SEE, wz);
         HLeadingPhoEcalIso.Fill(EcalIso, wz);
         HLeadingPhoHcalIso.Fill(HcalIso, wz);
         HLeadingPhoTrackIso.Fill(TrackIso, wz);
         HLeadingPhoIso.Fill(EcalIso + HcalIso + TrackIso, wz);

         Selected = true;
         if(SEE > 0.010392 || SEE < 0)
            Selected = false;
         if(EcalIso + HcalIso + TrackIso < 2.099277)
            Selected = false;

         HLeadingPhoSelected.Fill(Selected, wz);

         if(Selected == true)
         {
            HSelectedPhoEt.Fill((*phoEt)[lP], wz);
            if((*phoEt)[lP] > 40)
               HSelectedPhoEta.Fill((*phoEta)[lP], wz);
            if((*phoEt)[lP] > 40)
               HSelectedPhoPhi.Fill((*phoPhi)[lP], wz);
         }
      }

      if(Selected == false)   // we didn't find a good photon
         continue;

      // Now we move on to choose the jets!
      for(int iJ = 0; iJ < nref; iJ++)
      {
         if((*jtpt)[iJ] < 30)
            continue;
         if((*jteta)[iJ] < -1.6 || (*jteta)[iJ] > 1.6)
            continue;
         if((*jteta)[iJ] < -1.3 && (*jtphi)[iJ] < -0.8 && (*jtphi)[iJ] > -1.7)
            continue;
         if((*jteta)[iJ] < 1.5 && (*jteta)[iJ] > -0.5 && (*jtphi)[iJ] < 1.0 && (*jtphi)[iJ] > 0)
            continue;
         if((*jteta)[iJ] < 1.6 && (*jteta)[iJ] > 1.5 && (*jtphi)[iJ] < -1.0 && (*jtphi)[iJ] > -1.5)
            continue;

         double PhotonJetDEta = (*jteta)[iJ] - (*phoEta)[lP];
         double PhotonJetDPhi = (*jtphi)[iJ] - (*phoPhi)[lP];
         if(PhotonJetDPhi > +M_PI)   PhotonJetDPhi = PhotonJetDPhi - 2 * M_PI;
         if(PhotonJetDPhi < -M_PI)   PhotonJetDPhi = PhotonJetDPhi + 2 * M_PI;
         double PhotonJetDR = sqrt(PhotonJetDEta * PhotonJetDEta + PhotonJetDPhi * PhotonJetDPhi);

         if(PhotonJetDR < 0.4)
            continue;

         double PhotonJetWTADPhi = (*phoPhi)[lP] - (*WTAphi)[iJ];
         if(PhotonJetWTADPhi > +M_PI)   PhotonJetWTADPhi = PhotonJetWTADPhi - 2 * M_PI;
         if(PhotonJetWTADPhi < -M_PI)   PhotonJetWTADPhi = PhotonJetWTADPhi + 2 * M_PI;

         double AcceptanceWeight = 1;
         // Ignore this for now!  TODO TODO TODO TODO TODO
         double wj = wz * AcceptanceWeight;

         HPhotonJetDPhi.Fill(PhotonJetDPhi, wj);
         HPhotonJetWTADPhi.Fill(PhotonJetWTADPhi, wj);
         if(PhotonJetDPhi < 0.875 * M_PI)   // back to back-ness
            continue;

         double DEta = (*jteta)[iJ] - (*WTAeta)[iJ];
         double DPhi = (*jtphi)[iJ] - (*WTAphi)[iJ];
         if(DPhi > +M_PI)   DPhi = DPhi - 2 * M_PI;
         if(DPhi < -M_PI)   DPhi = DPhi + 2 * M_PI;
         double DR = sqrt(DEta * DEta + DPhi * DPhi);

         HSelectedJetPT.Fill((*jtpt)[iJ], wj);
         HSelectedJetEta.Fill((*jteta)[iJ], wj);
         HSelectedJetPhi.Fill((*jtphi)[iJ], wj);
         HSelectedJetDEta.Fill(DEta, wj);
         HSelectedJetDEtaWide.Fill(DEta, wj);
         HSelectedJetDPhi.Fill(DPhi, wj);
         HSelectedJetDPhiWide.Fill(DPhi, wj);
         HSelectedJetDR.Fill(DR, wj);
      }
   }

   Bar.Update(EntryCount);
   Bar.Print();
   Bar.PrintLine();

   HPTHatMin.Write();
   HPTHatMax.Write();
   HPTHat.Write();
   HVzNoWeight.Write();
   HVz.Write();
   HNGenJet.Write();
   HNGenJet30.Write();
   HNGenJet40.Write();
   HGenJetPT.Write();
   HGenJetEta.Write();
   HGenJetPhi.Write();
   HNJet.Write();
   HNJet30.Write();
   HNJet40.Write();
   HRefJetPT.Write();
   HRefJetEta.Write();
   HRefJetPhi.Write();
   HJetPT.Write();
   HJetEta.Write();
   HJetPhi.Write();
   HJetWTAEta.Write();
   HJetWTAPhi.Write();
   HJetDEta.Write();
   HJetDPhi.Write();
   HJetDR.Write();
   HNPho.Write();
   HNPho40.Write();
   HNPho60.Write();
   HPhoEta.Write();
   HPhoEt.Write();
   HPhoPhi.Write();
   HPhoHoverE.Write();
   HLeadingPhoFound.Write();
   HLeadingPhoEt.Write();
   HLeadingPhoEta.Write();
   HLeadingPhoPhi.Write();
   HLeadingPhoSigmaIEtaIEta.Write();
   HLeadingPhoEcalIso.Write();
   HLeadingPhoHcalIso.Write();
   HLeadingPhoTrackIso.Write();
   HLeadingPhoIso.Write();
   HLeadingPhoSelected.Write();
   HSelectedPhoEt.Write();
   HSelectedPhoEta.Write();
   HSelectedPhoPhi.Write();
   HPhotonJetDPhi.Write();
   HPhotonJetWTADPhi.Write();
   HSelectedJetPT.Write();
   HSelectedJetEta.Write();
   HSelectedJetPhi.Write();
   HSelectedJetDEta.Write();
   HSelectedJetDEtaWide.Write();
   HSelectedJetDPhi.Write();
   HSelectedJetDPhiWide.Write();
   HSelectedJetDR.Write();

   FOutput.Close();
   FInput.Close();

   return 0;
}

double GetWeight(double vz, vector<double> &p)
{
   static TF1 F("f", "(gaus(0))/(gaus(3))");
   F.SetParameters(p[0], p[1], p[2], p[3], p[4], p[5]);

   return F.Eval(vz);
}

