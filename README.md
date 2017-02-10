# Technology-classification-dataset
RSSI measurement of Wi-Fi, LTE and DVB-T captured at various locations of Ghent, Belgium. 

The RSSI of the signals are captured for Wi-Fi, LTE, DVB-T and all the measurements
are conducted in an office building of 12x80m. To increase the diversity of
signal strength, the measurement locations are placed on the north, east, and
west side of the building respectively. On each day and at each location, 10
traces are collected per technology. A trace contains 1×106 IQ
samples, obtained by USRP for a duration of 1 second, at the ADC sample rate of
1 MHz. The characteristics of the signals are described below: 

Wi-Fi (IEEE 802.11a/g): Signals transmitted in random bursts, modulated with
constant amount of carriers;

LTE: Signal transmitted in very fine and regular intervals, modulated with constant
amount of carriers;
Dataset description

DVB-T: Signals transmitted continuously and modulated with constant amount of carriers

More precisely, the Wi-Fi signal is captured in an office environment, including two
access points at 5540 MHz (channel 108), and on average 20 associated work
stations. The LTE signal is captured from a nearby base station, operating in
FDD mode at 806 MHz, around the Ghent area of Belgium. Finally, the DVB-T
signal is collected from the local TV broadcasting station at 482 MHz.

An Anritsu MS 2690A spectrum analyzer is used to capture samples of each of the
aforementioned signal types. The samples are collected at the rate of 10 MHz
for a duration of 1 second. The RSSI is calculated using Equation 1 for N =
200. In total 50 k RSSI values are computed. 

Script
MATLAB
script for plotting the spectrogram of the signals can be found here:
    
    %% low rate 
    sample_total = 1100000 ;
    sample_extra = 100000 ;
    addpath ../mfunctions/ ;
    filename =
    'wf_g80_uz_f5240MHz_r2.bin' ; 
    rawsp = read_complex(  filename, sample_total, 'float') ; 
    islinear = 0 ;
    if (islinear == 0)
        rssiarr =
    rssi_array_t(rawsp(sample_extra:end),20,islinear);
       
    [stat,interval]=hist_estimation(rssiarr,200,1,-80,-20);
    else
        rssiarr =
    rssi_array_t(rawsp(sample_extra:end),20,islinear);
        [stat,interval]=hist_estimation(rssiarr,10,1,0,2.2);
    end
    figure;
    bar(interval,stat)
    xlabel('RSSI
    (dB)','fontsize',18)
    ylabel('Normalized
    histogram','fontsize',18)
     
    grid on
    %% high rate spectrogram 
    fs = 10e6 ;
    fftsize = 256 ;
    nfft =
    floor((sample_total-sample_extra)/fftsize);
    specmatrix = zeros(nfft,
    fftsize); 
    data =
    rawsp(sample_extra+1:end);
    for i = 1 : nfft
        d = data((i-1)*fftsize+1:i*fftsize);
        specmatrix(i,:) = 10*log10(
    abs(fftshift( fft(d) ) ) )-30;
        %specmatrix(i,:) =  abs(fftshift( fft(d) ) ) ;
    end
    figure; 
    imagesc(specmatrix)
    caxis([-40 -25])
    
    
   
  
  
 


