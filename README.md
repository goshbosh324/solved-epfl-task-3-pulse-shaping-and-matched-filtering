Download Link: https://assignmentchef.com/product/solved-epfl-task-3-pulse-shaping-and-matched-filtering
<br>
Up to now we have only considered the transmission of complex data symbols over an AWGN channel. Of course, in reality there is no such thing as a complex symbol coming out of the antenna. Instead, the symbols have to be transformed into a time-continuous waveform. This transformation is called <em>modulation</em>.

<h1>3.1     Modulation</h1>

<h2>3.1.1    Transmission at Radio Frequency (RF)</h2>

The principle of digital communication systems is that only a finite number <em>M </em>of different symbols can be transmitted. Each symbol <em>a<sub>i </sub></em>is mapped onto a pulse <em>g<sub>i</sub></em>(<em>t</em>), <em>i </em>= 1<em>,…,M</em>. Since only finitely many pulses are possible, the receiver can recover the transmitted data if the signal is not too heavily distorted by the channel.

In this lab course, we only consider <em>linear </em>modulation schemes, where the pulses <em>g<sub>i</sub></em>(<em>t</em>) can be expressed as scaled versions of the same base pulse: <em>g<sub>i</sub></em>(<em>t</em>) = <em>a<sub>i</sub>g</em>(<em>t</em>). Hence, the magnitude of the data symbol determines the amplitude of the transmitted pulse, and the argument determines its phase. (An example of a <em>nonlinear </em>modulation scheme is frequency shift keying, where the information lies in the frequency of the transmitted signal.)

Let <em>T </em>denote the duration of one data symbol, i.e., a new pulse is transmitted each <em>T </em>seconds. Note that the pulse <em>g</em>(<em>t</em>) can still be longer than <em>T</em>; the pulses are allowed to overlap. The signal <em>s</em>(<em>t</em>) can then be written as the superposition of the pulses, scaled by the data symbols and delayed by multiples of <em>T</em>:

<em>.                                          </em>(3.1)

The signal <em>s</em>(<em>t</em>) is called a complex baseband signal, because it is complex-valued and its spectrum is centered at <em>f </em>= 0. In order to transmit the signal over the air, it must be transformed into a real-valued signal and its spectrum must be shifted to a carrier frequency <em>f<sub>c </sub></em>≫ <em>B</em>, where

<em>e</em>2<em>πjf</em><em>c</em><em>t                                                                                                    </em><em>e</em>−2<em>πjf</em><em>c</em><em>t</em>

Figure 3.1: Block diagram of a digital transmission system

3.2: PulseShapingFilters                                                                                    19

<table width="482">

 <tbody>

  <tr>

   <td width="34"><em>a</em>[<em>n</em>]</td>

   <td rowspan="2" width="56">Include zeros</td>

   <td width="34"><em>a</em>[<em>k</em>]</td>

   <td rowspan="2" width="56"><em>g</em>[<em>k</em>]</td>

   <td width="34"><em>s</em>[<em>k</em>]</td>

   <td rowspan="2" width="56">Channel model</td>

   <td width="34"><em>r</em>[<em>k</em>]</td>

   <td rowspan="2" width="56"><em>g</em><sub>MF</sub>[<em>k</em>]</td>

   <td width="34"><em>z</em>[<em>k</em>]</td>

   <td rowspan="2" width="56">Decimator</td>

   <td width="33"><em>z</em>[<em>n</em>]</td>

  </tr>

  <tr>

   <td width="34"> </td>

   <td width="34"> </td>

   <td width="34"> </td>

   <td width="34"> </td>

   <td width="34"> </td>

   <td width="33"> </td>

  </tr>

 </tbody>

</table>

Figure 3.2: Equivalent baseband system

<em>B </em>is the bandwidth of the signal. This operation can be written as follows:

<em>s</em>RF(<em>t</em>) = Re{<em>s</em>(<em>t</em>)<em>e</em>2<em>πjf</em><em>c</em><em>t</em>}

(3.2) = Re{<em>s</em>(<em>t</em>)}cos(2<em>πf<sub>c</sub>t</em>) − Im{<em>s</em>(<em>t</em>)}sin(2<em>πf<sub>c</sub>t</em>)<em>.</em>

The real part of the signal is modulated onto a cosine wave, and the imaginary part onto a sine wave. Due to the orthogonality of sin and cos, the two signals can be separated by the receiver.

The receiver converts the received signal <em>r</em><sub>RF</sub>(<em>r</em>) back into the baseband by multiplying it with <em>e</em><sup>−2<em>πjf</em></sup><em><sup>c</sup></em><em><sup>t </sup></em>and filtering the result with a lowpass filterin orderto eliminate the spectral components around 2<em>f<sub>c</sub></em>. The baseband signal <em>r</em>(<em>t</em>) is then sampled with the sampling rate 1<em>/T<sub>s</sub></em>, so that the remaining signal processing can be done by digital processors. The sampling duration is chosen to be shorter than the symbol duration, <em>T<sub>s </sub></em>= <em>T/L</em>, where <em>L </em>is the oversampling factor. We will explain later why the signal must be oversampled. The remaining task of the receiver is now to recover the transmitted data by means of the received data samples <em>r</em>[<em>k</em>]. (We use the time index <em>k </em>in order to denote samples of duration <em>T<sub>s</sub></em>, and the index <em>n </em>for data symbols of duration <em>T</em>).

<h2>            3.1.2    Equivalent Baseband Model</h2>

We have seen that a description of the received samples is quite inconvenient if the digital/analog conversion and the upconversion to radio frequency is taken into account. Fortunately, it is possible to describe the whole system as a time-discrete model in the complex baseband (as long as one is not interested in the effects of the RF components, of course).

A block diagram of this simplified model is shown in Figure 3.2. First, the symbol stream is converted to sample rate by inserting <em>L </em>− 1 zeros between the symbols. (This implies that <em>L </em>is an integer, which can generally be assumed. In the lab, we use an oversampling factor of <em>L </em>= 4.) The sequence <em>a</em>[<em>k</em>] is then processed by the pulse shaping filter <em>g</em>[<em>k</em>]. The time-discrete transmitted signal <em>s</em>[<em>k</em>] is then transmitted over a time-discrete baseband channel and filtered by the receiver filter <em>g</em><sub>MF</sub>[<em>k</em>]. Now the sample stream can be converted back to symbol rate by simply discarding three out of four samples. If the channel is an AWGN channel, which we assume up to now, the received symbols can then be written as <em>z</em>[<em>n</em>] = <em>a</em>[<em>n</em>]+<em>w</em><sup>′</sup>[<em>n</em>], where <em>w</em><sup>′</sup>[<em>n</em>] is the filtered noise.

<h1>            3.2     Pulse Shaping Filters</h1>

<h2>            3.2.1    Ideal Lowpass</h2>

We have not yet discussed the question how the pulse <em>g</em>(<em>t</em>) (or its time discrete version <em>g</em>[<em>k</em>] = <em>g</em>(<em>kT<sub>s</sub></em>)) should look like. In order to use the available spectrum as efficiently as possible, we would like to use a pulse whose spectrum is as narrow as possible. In order to transmit at a symbol rate <em>R </em>= 1<em>/T</em>, the bandwidth <em>B </em>of the signal must be at least as large as <em>R</em>. The

Chapter3: PulseShapingandMatchedFiltering

optimum filter would therefore be an ideal lowpass with the spectrum <em>G</em>(<em>f</em>) = rect(<em>f/R</em>), whose impulse response is known to be <em>g</em>(<em>t</em>) = si(<em>πt/T</em>), with si(<em>x</em>) = sin(<em>x</em>)<em>/x</em>.

With an ideal lowpass, the transmitted signal can be written as

<em>.                                     </em>(3.3)

It is important to note that <em>g</em>(<em>t</em>) becomes zero at multiples of the symbol duration <em>T</em>, except of <em>t </em>= 0:

<em>g</em>(<em>nT</em>) = 0     ∀<em>n </em>6= 0<em>.                                               </em>(3.4)

This is called the Nyquist criterion. If we sample this signal at the correct time instances <em>t </em>= <em>nT</em>, only the <em>n</em>-th pulse contributes to this sample, and we have recovered the transmitted symbols (except of noise).

The ideal lowpass however suffers from the drawback that the impulse response converges quite slowly towards zero. If we want to implement <em>g</em>[<em>k</em>] as a digital <em>finite impulse response </em>(FIR) filter, we need to implement many filter taps before the impulse response can be considered to be zero. (Strictly speaking, the impulse response of the ideal lowpass is infinitely long, so we can always only approximate its behavior). Another problem is that we must sample the signal exactly at the ideal time instants. If we have a slight timing error, as will always be the case in practical systems, the contribution of the other pulses is not zero anymore and our received symbols are distorted by <em>inter-symbol interference </em>(ISI). (The timing problem will be discussed in the next chapter.) Due to these two reasons, we are looking for a filter that still fulfills the Nyquist criterion, but whose impulse response converges to zero more rapidly.

<h2>3.2.2    Raised Cosine Filter</h2>

Such a filter is the <em>raised cosine </em>(RC). Its spectrum is given as

<em> .                                                                                     </em>(3.5)

The parameter <em>α </em>is the rolloff factor and determines the filter bandwidth. The two-sided bandwidth of the RC is <em>B </em>= <em>R</em>(1+<em>α</em>). Thus, the bandwidth of the RC is by a fraction of <em>α </em>larger than that of the ideal lowpass. Figure 3.3 shows the transfer function and the impulse response of the raised cosine filter for <em>α </em>∈ {0<em>.</em>2<em>,</em>0<em>.</em>5<em>,</em>0<em>.</em>8}. We see that the bandwidth increases with increasing rolloff factor, but the impulse response converges faster to zero. For <em>α </em>= 0, the raised cosine becomes an ideal lowpass. Note that the raised cosine fulfills the Nyquist criterion, i.e. becomes zero at multiples of the symbol duration.

<h2>3.2.3    Matched Filtering and Root Raised Cosine Filter</h2>

In the receiver, the signal is filtered by another filter. It can be shown that in case of an AWGN channel, the SNR after the receive filter is maximized if this filter is the complex conjugated and reverted (in time direction) transmit filter [2]:

<em>g</em><sub>MF</sub>(<em>t</em>) = <em>g</em><sup>∗</sup>(−<em>t</em>)<em>.                                                     </em>(3.6)

3.2: PulseShapingFilters                                                                                    21

−1     −0.5            0                  0.5              1                  −5               0                  5 <em>f/R       t/T</em>

(a)                                                                                       (b)

Figure 3.3: Transfer function (a) and impulse response (b) of the raised cosine filter

This filter is matched to the transmit filter and is therefore called the <em>matched filter </em>(MF). The total effective filter of the transmission system is then the combination of transmit and receive filter <em>g</em>(<em>t</em>) ∗ <em>g</em><sub>MF</sub>(<em>t</em>), where ∗ denotes convolution. It is important to note that this effective filter (and not the individual filters) must fulfill the Nyquist criterion. We can achieve this if both filters have a transfer function that is equal to the square root of that of the raised cosine filter. Such a filter is therefore called a <em>root raised cosine </em>(RRC). The combination of both RRC filters then becomes a raised cosine and thus fulfills the Nyquist criterion. Furthermore, since the filters are real-valued and symmetric, the RRC is its own matched filter.

The impulse response of the RRC filter is

<em>g</em><sub>RRC</sub><em>.                                                                                                      </em>(3.7)

Note that si(.

<h2>            3.2.4    Summary</h2>

When we use the RRC both as pulse shaping filter and matched filter, the effective filter fulfills the Nyquist criterion (3.4) and makes ISI-free reception possible. Also, the RRC converges faster to zero than an ideal lowpass and therefore suffers less from the drawbacks of the ideal lowpass discussed above.

The output of the matched filter can be written as

(3.8)

or in time-discrete samples as

<em>,                    </em>(3.9)

Chapter3: PulseShapingandMatchedFiltering

where the noise term <em>w</em><sup>′</sup>[<em>k</em>] denotes the AWGN <em>w</em>[<em>k</em>] after the matched filter. Due to (3.4), every <em>L</em>-th sample corresponds to a data symbol, <em>z</em>[<em>k </em>= <em>nL</em>] = <em>g</em><sub>RC</sub>[0]<em>a</em>[<em>n</em>] (in the following we assume that the filters are normalized so that <em>g</em><sub>RC</sub>[0] = 1) and hence the transmitted data can easily be recovered.

Unfortunately, (3.9) only holds if the receiver is perfectly synchronized to the incoming signal, which is never the case in a real system. In the next chapters, we will therefore examine some synchronization algorithms.

<h1>3.3    Your Tasks</h1>

<ol>

 <li>For the first task we provide an already shaped signal on the website. The system model is slightly different as we now use a oversampling factor of <em>L </em>= 4. The pulses are RRC shaped with a roll-off factor of <em>α </em>= 0<em>.</em>22. Add an AWGN to the system. Afterwards write a matched filter (MF) to demodulate the pulses. Demap the symbols and decode the image. Keep the number of receive filter taps as a variable. The frame synchronization that you implemented in the previous exercise <strong>can not </strong>be used with oversampled signals. For this reason, a modified frame synchronization function is provided on the website.</li>

 <li>Now implement a complete baseband transmission system to measure the BER:</li>

</ol>

i.) Generate a random bitstream. ii.) Convert to QPSK symbols. iii.) Upsample by a factor <em>L </em>= 4. iv.) Shape RRC pulses with a roll-off factor of <em>α </em>= 0<em>.</em>22 and 41 filter taps.

v.) Add AWGN. vi.) Apply matched filter (MF) with a variable number of filter taps. vii.) Revert upsampling (i.e., downsample by a factor <em>L </em>= 4). viii.) Demap the symbol stream. ix.) Calculate the BER.

A longer FIR receive filter (i.e., a filter having more taps) will lead to a lower bit error rate, but will also require more multiplications, which increases the energy consumption of the (battery driven) device. Your task is to determine the minimum length of the MF that exceeds a specified BER performance. By comparing the transmitted bits with your received bits, calculate the BER at an SNR of 8 dB for different MF lengths. Choose the minimum MF length so that the BER is better than 7 · 10<sup>−3</sup>, i.e. the implementation loss is less than 0.2 dB (see Figure 1.5).<sup>1</sup>

<strong>Note: </strong>Convolutions introduce heads and tails. Take special care to have a correct symbol alignment for BER calculations.