#include <iostream>
#include <vector>
using namespace std;

#include "TFile.h"
#include "TH1D.h"
#include "TCanvas.h"
#include "TKey.h"
#include "TList.h"
#include "TLegend.h"

#include "SetStyle.h"
#include "CommandLine.h"
#include "PlotHelper4.h"

int main(int argc, char *argv[])
{
   SetThesisStyle();

   vector<int> Colors = GetPrimaryColors();

   CommandLine CL(argc, argv);

   vector<string> Input = CL.GetStringVector("Input");
   vector<string> Label = CL.GetStringVector("Label");
   string Output = CL.Get("Output");

   if(Label.size() < Input.size())
      Label.insert(Label.end(), Input.size() - Label.size(), "No label");

   int N = Input.size();
   vector<TFile *> Files(N);
   for(int i = 0; i < N; i++)
      Files[i] = new TFile(Input[i].c_str());

   PdfFileHelper PdfFile(Output);
   PdfFile.AddTextPage("MC Comparisons");

   TList *List = Files[0]->GetListOfKeys();
   TIter Next(List);

   TKey *Object = nullptr;
   while((Object = (TKey *)Next()))
   {
      string HistogramName = Object->GetName();

      if(HistogramName == "HPTHatMin")   continue;
      if(HistogramName == "HPTHatMax")   continue;

      PdfFile.AddTextPage(HistogramName);

      TCanvas Canvas;
      TLegend Legend(0.5, 0.8, 0.8, 0.6);
      Legend.SetTextFont(42);
      Legend.SetTextSize(0.035);
      Legend.SetBorderSize(0);
      Legend.SetFillStyle(0);
      for(int i = 0; i < N; i++)
      {
         TH1D *H = (TH1D *)Files[i]->Get(HistogramName.c_str());
         H->SetStats(0);
         H->SetLineWidth(2);
         H->SetLineColor(Colors[i]);
         H->SetMarkerStyle(20);
         H->SetMarkerColor(Colors[i]);
         Legend.AddEntry(H, Label[i].c_str(), "pl");

         if(i == 0)
            H->DrawNormalized();
         else
            H->DrawNormalized("same");
      }
      Legend.Draw();
      PdfFile.AddCanvas(Canvas);

      Canvas.SetLogy();
      for(int i = 0; i < N; i++)
      {
         TH1D *H = (TH1D *)Files[i]->Get(HistogramName.c_str());
         if(i == 0)
            H->DrawNormalized();
         else
            H->DrawNormalized("same");
      }
      Legend.Draw();
      PdfFile.AddCanvas(Canvas);
   }

   PdfFile.AddTimeStampPage();
   PdfFile.Close();

   for(int i = 0; i < N; i++)
   {
      if(Files[i] == nullptr)
         continue;
      Files[i]->Close();
      delete Files[i];
   }

   return 0;
}


