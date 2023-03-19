# JS_Inflator_to_VST2_VST3

JS Inflator, the copy of Sonox Inflator, in vstsdk

<img src="https://github.com/Kiriki-liszt/JS_Inflator_to_VST2_VST3/raw/main/screenshot.png"  width="200"/>



Windows only. No Mac nor Linux compatible.  

<img src="https://github.com/Kiriki-liszt/JS_Inflator_to_VST2_VST3/raw/main/VST_Compatible_Logo_Steinberg_with_TM.png"  width="100"/>


VSTSDK 3.7.7 used  
VSTGUI 4.12 used  

Built as VST3, but compatible to VST2, too.  

This is my first attempt with VSTSDK & VSTGUI.  
Some codes might be inefficent...  

## Version logs

v1.0.0: intial try.  
v1.1.0: VuPPM meter change(mono -> stereo, continuous to discrete), but not complete!  
v1.2.0: VuPPM meter corrected!  
v1.2.1: Channel configuration corrected. probably a bug fix for crashing sometimes.  
v1.3.0: Curve knob fixed!!! and 32FP dither by airwindows.  
v1.4.0: Oversampling up to x8 now works! DPC works.  

## What I've learned

* Volumefader  

For someone like me, wondering how to use volume fader;  
Use a RangeParameter!  
param as normalized parameter[0.0, 1.0],  
dB as Plain value,  
gain as multiplier of each samples.  
For normParam to gain, check ~process.cpp  

ex)  
| param 	| dB  	| gain 	|
|-------	|-----	|------	|
| 0.0   	| -12 	| 0.25 	|
| 0.5   	| 0   	| 1    	|
| 1.0   	| +12 	| 4    	|  

| param 	| dB  	| gain 	|
|-------	|-----	|------	|
| 0.0   	| -12 	| 0.25 	|
| 0.5   	| -6  	| 0.5  	|
| 1.0   	| 0   	| 1    	|  

* Resampling  

Generally, the process goes as:  

1. doubling samples  
2. LP Filtering  
3. ProcessAudio  
4. LP Filtering  
5. reducing samples  

Filter choice is most critical, I think.  
HIIR code used Polyphase Filter, which is minimun phase filter.  
Has some letency, has some phase issue, but both very low.  

* Latency Reporting  

Naive implementaiton could use 'sendTextMessage' and 'receiveText' pair.  

## references

1. RC Inflator
<https://forum.cockos.com/showthread.php?t=256286>  


2. HIIR resampling codes by 'Laurent De Soras'.  
<http://ldesoras.free.fr/index.html>  

## Todo

* Preset save & import menu bar.  
* malloc/realloc/free to new/delete change.  
