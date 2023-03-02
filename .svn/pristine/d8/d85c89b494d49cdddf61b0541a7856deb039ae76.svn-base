#include <iostream>
#include <vector>
using namespace std;

#include "TFile.h"
#include "TH1D.h"
#include "TKey.h"
#include "TList.h"

#include "CommandLine.h"

int main(int argc, char *argv[])
{
   CommandLine CL(argc, argv);

   vector<string> Input = CL.GetStringVector("Input");
   string Output = CL.Get("Output");

   int N = Input.size();

   vector<TFile *> Files(N);
   for(int i = 0; i < N; i++)
      Files[i] = new TFile(Input[i].c_str());

   vector<double> Weight(N);
   Weight[0] = 1;
   for(int i = 1; i < N; i++)
   {
      double W0 = ((TH1D *)Files[i-1]->Get("HPTHatMax"))->Integral();
      double W1 = ((TH1D *)Files[i]->Get("HPTHatMin"))->Integral();

      // cout << i << " " << W0 << " " << W1 << endl;
      Weight[i] = Weight[i-1] * W0 / W1;
   }

   TFile OutputFile(Output.c_str(), "RECREATE");

   TList *List = Files[0]->GetListOfKeys();
   TIter Next(List);

   TKey *Object = nullptr;
   while((Object = (TKey *)Next()))
   {
      string HistogramName = Object->GetName();
      TH1D *H = (TH1D *)Files[0]->Get(HistogramName.c_str())->Clone(HistogramName.c_str());
      for(int i = 1; i < N; i++)
         H->Add(((TH1D *)Files[i]->Get(HistogramName.c_str())), Weight[i]);
      H->Write();
   }

   OutputFile.Close();

   for(int i = 0; i < N; i++)
   {
      if(Files[i] != nullptr)
      {
         Files[i]->Close();
         delete Files[i];
      }
   }
   
   return 0;
}




