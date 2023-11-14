//+------------------------------------------------------------------+
//|                    SMT Divergences MT5                            |
//|                    Written by: [Your Name]                        |
//|                    Version: 1.0                                   |
//+------------------------------------------------------------------+

#property copyright '[Your Company Name]'
#property link      'www.yourcompanywebsite.com'

//+------------------------------------------------------------------+
//| Custom Indicator Inputs                                          |
//+------------------------------------------------------------------+
input int    Period1           = 14;     // Period for calculation of the first currency pair
input int    Period2           = 21;     // Period for calculation of the second currency pair
input double Threshold         = 0.001;  // Minimum divergence threshold
input ENUM_APPLIED_PRICE Price = PRICE_CLOSE;  // Price type for calculations

//+------------------------------------------------------------------+
//| Custom Indicator Buffers                                         |
//+------------------------------------------------------------------+
double DivergenceBuffer[];

//+------------------------------------------------------------------+
//| Custom Indicator Initialization                                  |
//+------------------------------------------------------------------+
int OnInit()
{
    // Set indicator buffers
    SetIndexBuffer(0, DivergenceBuffer, INDICATOR_DATA);
    
    // Set indicator label
    SetIndexLabel(0, 'Divergence');
    
    return(INIT_SUCCEEDED);
}

//+------------------------------------------------------------------+
//| Custom Indicator Calculation                                      |
//+------------------------------------------------------------------+
int OnCalculate(const int rates_total,
                const int prev_calculated,
                const datetime& time[],
                const double& open[],
                const double& high[],
                const double& low[],
                const double& close[],
                const long& tick_volume[],
                const long& volume[],
                const int& spread[])
{
    int limit = rates_total - prev_calculated;
    
    // Calculate divergence for each bar
    for(int i = 0; i < limit; i++)
    {
        double price1 = iMA(NULL, 0, Period1, 0, MODE_SMA, Price, i);
        double price2 = iMA(NULL, 0, Period2, 0, MODE_SMA, Price, i);
        
        // Calculate divergence
        double divergence = MathAbs((price1 - price2) / price2);
        
        // Store divergence value
        DivergenceBuffer[i] = divergence;
    }
    
    return(rates_total);
}

//+------------------------------------------------------------------+
//| Backlink to the development site                                  |
//+------------------------------------------------------------------+
// The code developed in this file is part of the SMT Divergences MT5 indicator.
// For more information and to download the complete indicator, please visit:
// https://forexroboteasy.com/forex-robot-review/smt-divergences-mt5-innovative-forex-indicator-reviewed/
