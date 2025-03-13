# Lab 2

## Exercise 1a: Coherent Detection

The formula for AM applied on a single tone signal (a signal that consists of a single frequency) is given by:

$s(t) = [A_c + A_m \cos(2\pi f_m t)] \cos(2\pi f_c t)$

**Parameters:**

$A_m =$ Message Amplitude<br>
$A_c =$ Carrier Amplitude<br>
$f_m =$ Message Frequency<br>
$f_c =$ Carrier Frequency 

**Coherent Demodulation:** Coherent demodulation involves multiplying the received AM signal with a synchronized carrier signal of the same frequency and phase. This process shifts the signal to baseband. A low-pass filter is then used to remove high-frequency components, leaving the original message signal for further processing.

**Envelope detection:** Envelope detection rectifies the AM signal to convert it to a unipolar form. A low-pass filter is then applied to smooth out the high-frequency components, capturing the signal's amplitude variations. Finally, the DC component is removed to extract the original message signal.

### Task 1: Add the block diagram to your logbook.
The block diagram for the coherent demoulation is shown:

![Coherent Demodulation Block Diagram](coherentdemodblock.png)

1. **Multiply AM Signal by Local Carrier**  
    Let the AM signal be $x(t) = [A + m(t)] \cos(\omega_c t)$.

    Multiply it by a local carrier $\cos(\omega_c t)$:
     $$
     x(t) \cdot \cos(\omega_c t) = [A + m(t)] \cos^2(\omega_c t).
     $$
   
2. **Apply Trig Identity**<br>
    Using $\cos^2(\theta) = \frac{1}{2} + \frac{1}{2}\cos(2\theta)$:
     $$
     [A + m(t)] \cos^2(\omega_c t)
     = \frac{A + m(t)}{2} + \frac{A + m(t)}{2} \cos(2\omega_c t).
     $$

3. **Low-Pass Filter**  
    A low-pass filter removes the high-frequency term $\cos(2\omega_c t)$, leaving:
     $$
     \frac{A + m(t)}{2}.
     $$
    This is essentially the baseband signal $m(t)$ plus a DC offset.

4. **AC/DC RMS (Offset Removal / Signal Measurement)**  
    The AC/DC RMS block can remove the DC offset or provide an amplitude measurement of the recovered signal, yielding a cleaner baseband output.

5. **Demodulated Output & PSD**  
    The final output labeled *Demod Signal Time* is the recovered message $m(t)$.
    A *PSD* (Power Spectral Density) block can be used to visualize the frequency content of the demodulated signal.

**Butterworth Filter**

The butterworth filter is a low pass filter which lets the frequencies below the cutoff frequency through with little attentuation and rolls off above that frequency, the higher the order, the steeper the roll off but introduces more delay.

### Task 2: Explain briefly the mathematical theory behind this demodulation technique. Moreover, why are we using a low pass filter and why do we have to get rid of the DC component? Why do you need to scale the message amplitude?

In coherent demodulation of Amplitude Modulation (AM), the received signal $r(t)$ can be represented as:

$$
r(t) = [A_c + m(t)] \cos(\omega_c t)
$$

where:
- $A_c$ is the amplitude of the carrier.
- $m(t)$ is the message (baseband) signal.
- $\omega_c$ is the angular frequency of the carrier.

To demodulate, the received signal is multiplied by a locally generated carrier signal (coherent carrier) of the same frequency and phase:

$$
y(t) = r(t) \cdot \cos(\omega_c t) = [A_c + m(t)] \cos^2(\omega_c t)
$$

Using the identity $\cos^2(x) = \frac{1 + \cos(2x)}{2}$, we have:

$$
y(t) = \frac{1}{2}[A_c + m(t)] + \frac{1}{2}[A_c + m(t)] \cos(2\omega_c t)
$$

This result has two main components:

- Baseband component:: $\frac{1}{2}[A_c + m(t)]$
- High-frequency component: $\frac{1}{2}[A_c + m(t)]\cos(2\omega_c t)$

**Purpose of the Low-Pass Filter**

After multiplication, a low-pass filter (LPF) is applied to isolate the baseband signal. The LPF removes the high-frequency component at frequency $2\omega_c$, leaving only:

$$
y_{\text{LPF}}(t) = \frac{1}{2}[A_c + m(t)]
$$

Without the LPF, the high-frequency term would remain, distorting the recovered message.

**Removing the DC Component**

The DC component ($\frac{A_c}{2}$) is a constant offset that does not contain information about the message. It must be removed because it shifts the signal's baseline, causing inaccuracies in signal interpretation and further signal processing. After DC removal, we obtain:

$$
y_{\text{out}}(t) = \frac{1}{2}m(t)
$$

**Scaling the Message Amplitude**

Finally, amplitude scaling is necessary because the demodulation process inherently reduces the amplitude of the message signal by a factor of $\frac{1}{2}$. Scaling restores the recovered message amplitude to match the original transmitted message, resulting in:

$$
m_{\text{recovered}}(t) = m(t)
$$

This step ensures accurate representation and further processing of the recovered signal.

## Exercise 1b: Envelope Detection

### Task 1: Explain briefly the mathematical theory behind this demodulation technique and all the steps implemented.

Envelope detection is a key technique for demodulating amplitude-modulated (AM) signals. The envelope of an AM signal contains the original modulating information and can be extracted using rectification followed by low-pass filtering.

The AM signal is given by:
   
$$ s(t) = [A + m(t)] \cos(\omega_c t) $$
   
   where:
$A$ is the carrier amplitude,
$m(t)$ is the message signal,
$\omega_c$ is the carrier frequency.

1. **Rectification:**  
   Rectification removes the negative half-cycles of the signal, leaving only positive values. Half-wave rectification introduces an amplitude loss, requiring pre-amplification.

2. **Low-Pass Filtering:**  
   A Butterworth filter is applied to smooth the rectified signal and extract the envelope. The cutoff frequency is chosen based on the bandwidth of the message signal.

3. **DC Component Removal:**  
   A subtraction of the DC component is done to remove the offset introduced by rectification.

### Task 2: Add the block diagram to your logbook.

![Envelope Detector Block Diagram](envelopedetectionlbock.png)

## Exercise 2: AM Simulation

### Task 1: Block Diagram

![Amplitude Modulation and Demodulation](exc2block.PNG)

This shows the usage of sub VIs using the previously completed VIS as a submodule in this top module VI. A single tone signal is input to the modulator which executes AM and is then passed to both coherent and envelope demodulation modules.

### Task 2: Observe the time domain and PSD representations of the coherent and envelope demodulated signals as the message signal amplitude changes from 1 to 4 with steps of 1. As you change the message signal amplitude do you observe any changes between the two modulation techniques? If yes, explain the reason.

*$A_m=1$ graph results*
![Results for A_m=1](exc2am_m1.PNG)

*$A_m=2$ graph results*
![Results for A_m=2](exc2am_m2.PNG)

*$A_m=3$ graph results*
![Results for A_m=3](exc2am_m3.PNG)

*$A_m=4$ graph results*
![Results for A_m=4](exc2am_m4.PNG)

**For envelope demodulation**
The message amplitude increases while the carrier amplitude remains at 1. This results in overmodulation where $m>1$ and causes distortions inthe demodulated wave. This is due to the envelope crossing zero amplitude line, it inverts and produces the distortion in the envelope-detected output.

**For coherent demodulation**
The resulting graph is largely unchanged for coherent demodulation since it uses a carrier at the receiver synchronized in frequency and phase to the original carrier and multiplies it. This allows it to measure both positive and negative instantaneous amplitudes, preserving the linear relationship between modulating signal and output.

## Exercise 3: AM Communications via USRP

### Task 1: Run the code and explain in your logbook how the transmitter and receiver work. Hint: the USRP does all the modulation and demodulation on its own so the modules only needs to provide the complex data for the USRP to modulate. At the receiver the USRP will return the demodulated data.


### Task 2: Transmit a single-tone message signal with frequency 5kHz and modulation index 1 (you may have to adjust the low cutoff frequency of the filter). Run the receiver, and plot the demodulated signal in time, and its PSD. Adjust the time and PSD plot to the relevant region so that the plot is not just a blur, and rename the axes appropriately. Add the plots to your logbook.


### Task 3: To observe the effect of noise in the demodulated signal, increase the receiver’s gain to 20 dB (your receiver will start to detect other weaker signals in addition to the transmitted signal), and adjust the X axis of the demodulated message (in time domain) to show values between 0 second (s) and 0.004 s. Then, change the modulation index (𝜇) value and observe the effects on the plots of the demodulated signal in both time and frequency. From what value of 𝜇 can noise be clearly noticed in the plots? Copy the image of the noisy demodulation into your logbook.

# Lab 3

## Exercise 1: FM Modulator

In frequency modulation (FM), the information in the message signal is conveyed through variations in the frequency of the modulated carrier signal. The general expression for an FM signal is:

$$s(t) = A_c \cos(2\pi f_c t + \theta_m(t))$$

where

$$\theta_m(t) = 2\pi k_f \int_{0}^{t} m(\tau) d\tau,$$

$m(t)$ represents the message signal, and $k_f$ is the frequency sensitivity. The instantaneous frequency of the modulated signal is given by:

$$f_i = f_c + k_f m(t)$$

An equivalent expression for the FM signal is:

$$s(t) = A_c \cos(2\pi f_c t) \cos(\theta_m(t)) - A_c \sin(2\pi f_c t) \sin(\theta_m(t))$$

The figure below illustrates a message signal (left) and its corresponding FM-modulated waveform (right). The modulation results in a varying frequency over time.

The frequency deviation is given by:

$$\Delta f = k_f m_f$$

where $m_f = \max |m(t)|$ represents the peak value of the message signal. The modulation index, which describes the extent of frequency variation relative to bandwidth, is defined as:

$$\mu = \frac{\Delta f}{B}$$

where $B$ denotes the message bandwidth.

**Observations about Frequency Modulation (FM)**

- As frequency deviation ($\Delta f$) increases, the density of the FM signal in the time domain becomes more spread out.  
- A larger $\Delta f$ also widens the FM signal’s power spectral density (PSD) bandwidth.  
- Carson’s rule estimates the FM bandwidth as $B_{FM} \approx 2 (\Delta f + B)$ where $B$ is the highest modulating (message) frequency.  


### Task 1: Set the parameters as in the following table and observe the changes in the modulated signal. 

| Parameter                     | Value           |
|--------------------------------|---------------|
| Message signal frequency       | 1 kHz         |
| Carrier signal frequency       | 10 kHz        |
| Sample rate                    | 200k          |
| Samples                        | 1k            |
| \( k_f \)                      | [500, 2000, 5000] |
| Message signal amplitude       | 1             |
| Carrier signal amplitude       | 1             |

There are differences in the time domain and frequency domain graphs of the demodulated wave result.

### Task 2: Explain the resulting changes in the FM signal and its PSD as you change $\Delta_f$. Add the graphs of the FM signal and its PSD to your logbook for different $\Delta_f$.

**Graphs for $k_f=500$**
![ $k_f=500$ ](task1kf500.png)
The FM signal here looks consistent and resembles the original carrier wave where the frequency sensitivity is smaller, $k_f=500$. There are three peaks, one of them being very large.

**Graphs for $k_f=2000$**
![ $k_f=2000$ ](task1kf2000.png)
A little more frequency variation can be seen in the FM signal. More peaks are seen as the sidebands involved in FM get more significant and their power distribution is bigger.

**Graphs for $k_f=5000$**
![ $k_f=5000$ ](task1kf5000.png)

For Δf = 500Hz

$$
μ = Δf / B = 500 / 1000 = 0.5
$$

For Δf = 2000Hz

$$
μ = Δf / B = 2000 / 1000 = 2
$$

For Δf = 5000Hz

$$
μ = Δf / B = 5000 / 1000 = 5
$$

When the frequency sensitivity is very high, there are rapid and large frequency variations in the time domain. The oscillations become more compressed and stretched as the frequency sweeps faster. The number of spectral peaks increase with a high $k_f$ and the signals energy is distributed among many frequencies.

### Task 3: Include the block diagram of your FM_Modulator.gvi in your logbook.

![Block diagram for fm modulator](task1block.png)

The circuit executes the equations written above for FM:

$$\theta_m(t) = 2\pi k_f \int_{0}^{t} m(\tau) d\tau,$$
$$s(t) = A_c \cos(2\pi f_c t) \cos(\theta_m(t)) - A_c \sin(2\pi f_c t) \sin(\theta_m(t))$$

## Exercise 2: FM Demodulator

### Task 1: Explain briefly the mathematical theory behind this demodulation technique.

**Frequency Modulation (FM) Overview**

In FM, the instantaneous frequency of the carrier is varied according to the message signal $m(t)$. The FM signal can be expressed as:

$$
s_{FM}(t) = A_c \cos \left( \omega_c t + k_f \int m(t) dt \right)
$$

where:
- $A_c$ is the carrier amplitude,
- $\omega_c$ is the carrier angular frequency,
- $k_f$ is the frequency sensitivity of the modulator,
- $m(t)$ is the message signal.

The instantaneous frequency $f_i(t)$ is given by:

$$
f_i(t) = \frac{1}{2\pi} \frac{d}{dt} \left( \omega_c t + k_f \int m(t) dt \right) = f_c + \frac{k_f}{2\pi} m(t)
$$

where $f_c = \frac{\omega_c}{2\pi}$ is the carrier frequency.

**Frequency Discrimination Approach**
FM demodulation using frequency discrimination involves two steps:

1. **Differentiation:**  
   The FM signal is differentiated to convert frequency variations into amplitude variations. Mathematically,

   $$
   \frac{d}{dt} s_{FM}(t) = -A_c \sin \left( \omega_c t + k_f \int m(t) dt \right) \cdot \left( \omega_c + k_f m(t) \right)
   $$

   The result is an amplitude-modulated signal where the envelope is proportional to the instantaneous frequency.

2. **Envelope Detection:**  
   Since the differentiated signal is an AM signal (containing both FM and AM components), an envelope detector is used to extract the amplitude, which corresponds to $m(t)$.

   The final recovered message signal is:

   $$
   m(t) \approx \frac{1}{k_f} \left( f_i(t) - f_c \right)
   $$

This method effectively converts frequency variations into amplitude variations, which can be easily extracted using an envelope detector.

### Task 2: Add the block diagram to your logbook.

![FM Demodulation Block](demodblock.png)

## Exercise 3: FM Simulation

### Task 1: Add the block diagram to your logbook.

![FM top module](task3block.png)

### Task 2: Set the parameters as in the above table. Observe the demodulated signal and attach the plots of the FM signal and the demodulated signal with their corresponding PSDs to your logbook (for the demodulated signal, you should also provide a zoomed version, without the initial transient, so that it is easy to observe the amplitude of the message).

![FM top graphs](task3graphs.png)

The demoulated wave looks very similar to the original message wave

## Exercise 4: FM Communications via USRP

### Task 1: Add the power spectrum graph for each of these cases into your logbook. Be sure to scale the horizontal axis so that each spectrum is visible clearly. Annotate your spectra to show the FM signal bandwidth (Carson’s rule). 

# Lab 4 

## Exercise 3: Differential Phase Shift Keying (DPSK)

### Task 1: Add the block diagram of your DPSK system to your logbook

![dpsk system block diagram](fullblock.png)

### Task 2: Run your code several times using the initial configuration used for the BPSK system. Explain your observation(s) on the BER performance of the system.

**Graph for $rxgain=0, txgain=0$**
![bervalue1](bertrial1.png)

**Graph for $rxgain=-15, txgain=-35$**
![bervalue2](bertrial2-15-35.png)

**Graph for $rxgain=-15, txgain=-37$**
![bervalue3](bertrial3-15-37.png)

**Graph for $rxgain=-15, txgain=-40$**
![bervalue4](bertrial3-15-40.png)



### Task 3: Compare the performance of BPSK and DPSK, and explain your findings.

The BPSK results had overall lower BER values as it is a coherent detection method and can have a low BER when it has an accurate reference carrier signal to demodulate. 

DPSK encodes information in the difference between successive symbols. The differential detection process makes DPSK inherently more susceptible to noise, because any noise in the previous symbol can degrade the detection of the current symbol. This may result in an error propagation resulting in about a 3 dB performance loss relative to coherent BPSK.

