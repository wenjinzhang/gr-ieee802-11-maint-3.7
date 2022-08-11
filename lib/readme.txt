The modifications here mainly extract a collection of meta data
along the frame detection and CSI parsing procedures,  append
them to either the 'wifi_start' or 'preamble' tagged message stream, 
and finally export these messages via an extra message bus labelled
as "out" in the parse_mac.cc module.  

All modifications are marked with '[X.Z. ...]' at the beginning and
end in each file. 


The added meta data include: 
* "sof":              a rough start-of-frame position from the sync_short module;
* "freq_offset":      coarse CFO from the sync_short module; 
* "frame_start":      refined frame start position from the sync_long module; 
* "freq_offset_long": fine CFO from the sync_long module; 
* "pre_in_f":         the 64-dim complex vector corresponding to the pre-equalized 
                      frequency-domain symbol in the L-LTF from the frame_equalizer
                      module. There should be a pair of them in each wifi frame,
                      with a "symbol_id" value as either 0 or 1; 
                  
* "bits_per_symbol",  "frame_symbols", etc.:  decoded L-SIG field info from the
                      frame_equalizer module; 

* "fc", "mac1", etc.: frame control and mac address info in parse_mac module

   
One quick way to see how they are added is to search for 'dict_add' in the corresponding
source files

Notes: 
1.  One can derive the CSI from the "pre_in_f" field in the messaged tagged as
"preamble", by flipping the phase of these pre-equalized frequency-domain values following
the pseurandom binary sequence shown in the variable LONG defined in equalizer/base.cc

2. Consult https://wiki.gnuradio.org/index.php/Message_Passing for additional explanation
on GNU Radio's message passing mechanism, and code examples on how to take advantage of this. 

3. The wifi receiver already includes a module for printing out frame error rates (FER) from
the decoding process.  One can follow this example to print out the additional tagged message
on pre-equalized CSI in a similar fashion. 

 


