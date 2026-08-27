# Lakomy et al. 2026

This code accompanies the analysis of in vivo calcium imaging experiments in **Lakomy et al (2026) : Acetylcholine drives astrocytic JAK2-STAT3 signaling to modulate male-to-female approach behavior.**


## **Overview** 

This experiment was designed to assess how astrocytic JAK2 activation affects vCA1 pyramidal neuron activity in male mice during social encounter with a non-estrus female.


## **Experimental design**

Male mice received vHPC injections of pyramidal neuron-targeted AAV expressing the genetically encoded calcium indicator GCaMP8m, together with astrocyte-targeted AAV expressing either constitutively active JAK2 (JAK2ca) or td-Tomato as a control. An optic fiber cannula was implanted in the vHPC to enable fiber photometry recordings of vCA1 pyramidal neuron calcium activity.
Calcium activity was recorded during sequential exposure to two conditions: an object and a non-estrus female. The analysis focuses on changes in normalized GCaMP8m fluorescence associated with these behavioral stimuli, with particular emphasis on the comparison between JAK2ca and control animals. A brief tail-suspension stimulation was also used as a positive control to assess recording and signal quality.


## **Key findings**
-	Object exposure: vCA1 pyramidal neuron calcium activity remained comparable to baseline in both JAK2ca and td-Tomato control groups.
-	Female exposure: First contact with a female induced a robust increase in vCA1 calcium activity in both groups. However, this response was reduced in JAK2ca mice compared with td-Tomato controls.
-	Signal quality control: A 10-second tail suspension induced a strong increase in vCA1 calcium activity with similar amplitude across groups, supporting the quality and reliability of the calcium recordings.


## **Fiber Photometry Analysis Pipeline**

Signal was acquired using 410 nm and 470 nm fluorescence channels.

The workflow includes signal preprocessing, photobleaching correction, movement artifact correction using the 410 nm control channel, peri-event z-score normalization, and quantification of stimulus-evoked responses.

1. **Data loading and preprocessing**

   - Loads the recording data.
   - Converts timestamps from milliseconds to seconds.
   - Removes the first 60 seconds of recording to exclude the initial fast photobleaching period.

2. **Signal filtering**

   - Applies a median filter to both 410 nm and 470 nm signals.
   - Applies a zero-phase, 4th-order Butterworth low-pass filter.

3. **Photobleaching correction**

   - Fits a double-exponential decay to the filtered 410 nm and 470 nm signals.
   - Removes the fitted decay from each signal to correct for photobleaching.

4. **Movement artifact correction**

   - Uses the corrected 410 nm control channel to predict movement-related fluctuations in the 470 nm signal through linear regression.
   - Subtracts the predicted artifact from the 470 nm signal to obtain the movement-corrected signal.
   - Correlations between the 410 nm and 470 nm signals are calculated before and after correction to assess the effect of the procedure.

5. **Peri-event analysis**

   - Extracts signal around predefined behavioral events.
   - Normalizes each event using a baseline period from (−15 to −10 s) and converts the signal to a z-score.
   
6. **Response quantification**

   - Calculates the mean z-scored response within predefined baseline and response windows.
   - Calculates the area under the curve (AUC) for both baseline and response periods.
   - Exports the resulting measurements to an Excel file.

## Input

The script expects a table containing :

* **Column 1:** Time
* **Column 3:** 410 nm control channel
* **Column 4:** 470 nm signal channel


## Output

The script generates:

* Quality-control plots comparing raw and filtered signals.
* Double-exponential photobleaching fits.
* Corrected and movement-corrected signals.
* Correlation plots before and after movement correction.
* Peri-event z-score plots.
* An Excel file containing the aligned peri-event z-score data.
* An Excel file containing mean response and AUC measurements.







