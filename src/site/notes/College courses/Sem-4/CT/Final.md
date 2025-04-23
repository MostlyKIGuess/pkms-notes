---
{"dg-publish":true,"permalink":"/college-courses/sem-4/ct/final/"}
---

![][./final.pdf]
# Chapter 1: Introduction

## Basic Communication System
*   **Goal:** Transfer information reliably and efficiently from a Source to a Destination across a Channel.
*   **Fundamental Blocks:** Source $\rightarrow$ Transmitter $\rightarrow$ Channel $\rightarrow$ Receiver $\rightarrow$ Destination.
*   **Transmitter:** Encodes source information into a signal suitable for the channel (modulation).
*   **Channel:** The physical medium (wire, fiber, air, etc.) that carries the signal. Introduces noise, distortion, attenuation.
*   **Receiver:** Processes the received (corrupted) signal to estimate the original information (demodulation, decoding).
*   **Transfer Medium:** Can be across space (real-time transmission) or time (storage).

## The Channel: Resource and Challenge
*   The channel is the **resource** enabling communication but presents **challenges**.

###  Channel Challenge: Limited Spectrum/Bandwidth
*   Physical media cannot support infinite frequencies.
*   Wireless spectrum is a scarce, regulated resource.
*   Wired channels exhibit frequency-dependent attenuation (e.g., high frequencies attenuated more in twisted pairs).
*   *Motivation:* Leads to the need for **bandwidth-efficient** communication techniques.

###  Channel Challenge: Noise
*   Unavoidable random perturbations corrupting the signal.
*   **Thermal Noise:** Due to random motion of electrons in conductors. Fundamental limit.
*   **Other Sources:** Interference (other transmitters), man-made noise (electronics), shot noise.
*   **AWGN Model:** Standard mathematical model. Assumes noise is:
    *   **Additive:** $y(t) = s(t) + n(t)$.
    *   **White:** Flat Power Spectral Density (PSD) $S_n(f) = N_0/2$ (two-sided). Noise power in bandwidth B is $P_N = N_0 B$.
    *   **Gaussian:** Noise amplitude follows a Gaussian distribution.
*   *Motivation:* Need for techniques robust to noise, leading to signal design and error control coding.

###  Channel Challenge: Distortion
*   **Attenuation:** Signal weakening with distance/frequency.
*   **Multipath (Wireless):** Signal arrives via multiple paths, causing constructive/destructive interference (fading) and delay spread (causing Inter-Symbol Interference - ISI).
*   *Motivation:* Requires equalization techniques at the receiver and robust modulation schemes.

###  Channel Challenge: Limited Capacity
*   **Shannon's Capacity Theorem:** Defines the maximum theoretical rate (bits/sec) for reliable communication over a noisy channel.
    $$ C = B \log_2 (1 + \text{SNR}) $$
*   Indicates a fundamental trade-off between bandwidth ($B$), power (via SNR), and data rate ($C$).
*   *Motivation:* Benchmarks performance and drives the search for efficient coding and modulation.

## Analog vs. Digital Communication
*   **Analog:** Information represented by continuous waveforms. Simple but degrades easily with noise.
*   **Digital:** Information represented by discrete symbols (bits). Enables sophisticated processing.
    *   **Why Digital?** (Sec 1.1.3): **Robustness** (regenerative repeaters clean noise), **Optimality** (Source-Channel Separation allows independent optimization), **Scalability** (networking, packet switching), **Flexibility** (DSP, error correction, compression, encryption).
*   **Digital System Blocks:** Source Coding (Compression) $\rightarrow$ Channel Coding (Error Control) $\rightarrow$ Modulation $\rightarrow$ Channel $\rightarrow$ Demodulation $\rightarrow$ Channel Decoding $\rightarrow$ Source Decoding.

---

# Chapter 2: Signals and Systems Fundamentals

## Signal Representations (Sec 2.2)
*   **Sinusoid:** Basic periodic signal $s(t) = A \cos(2\pi f_c t + \theta)$.
*   **Complex Exponential:** $s(t) = \alpha e^{j2\pi f_c t}$. Powerful tool, eigenfunction of LTI systems.
*   **Relation:** $A \cos(2\pi f_c t + \theta) = \text{Re}\{ A e^{j\theta} e^{j2\pi f_c t} \}$.

## Inner Product, Energy, Power (Sec 2.2)
*   **Inner Product:** $\langle s, r \rangle = \int s(t) r^*(t) dt$. Measures projection/similarity.
*   **Energy:** $E_s = \|s\|^2 = \langle s, s \rangle$.
*   **Power:** $P_s = \overline{|s(t)|^2}$.

## Fourier Analysis (Sec 2.4, 2.5)
*   Transforms signals between time and frequency domains.
*   **Key Pairs:** $\delta(t) \leftrightarrow 1$; $e^{j2\pi f_0 t} \leftrightarrow \delta(f-f_0)$; $\text{rect}(t/T) \leftrightarrow T\text{sinc}(fT)$.
*   **Convolution Theorem:** $x(t) * h(t) \leftrightarrow X(f)H(f)$. *Fundamental for LTI systems.*
*   **Parseval's Theorem:** $\int |u(t)|^2 dt = \int |U(f)|^2 df$. *Relates energy in time and frequency.*

## Spectral Density and Bandwidth (Sec 2.6)
*   **ESD (Energy Signals):** $\mathcal{E}_u(f) = |U(f)|^2$.
*   **PSD (Power Signals/WSS Processes):** $S_X(f) = \mathcal{F}\{R_X(\tau)\}$. Describes power distribution vs. frequency.
*   **Bandwidth:** Defines frequency occupancy. Various definitions (e.g., 99% power containment).

## Complex Baseband Representation (Sec 2.8)
*   **Motivation:** Simplifies analysis of passband signals/systems by shifting the spectrum to baseband and using complex notation.
*   **Representation:** Real passband $u_p(t)$ $\leftrightarrow$ Complex baseband $u(t)$.
    $$ u_p(t) = \text{Re}\{ u(t) e^{j2\pi f_c t} \} $$
*   **I/Q Components:** $u(t) = u_c(t) + j u_s(t)$. $u_p(t) = u_c(t) \cos(2\pi f_c t) - u_s(t) \sin(2\pi f_c t)$.
*   **Spectrum:** $U_p(f) = \frac{1}{2} [U(f - f_c) + U^*(-f - f_c)]$. The spectrum of $u(t)$ is $U(f)$.
*   **Power/Energy:** $P_{u_p} = \frac{1}{2} P_u$. Conserves total energy/power (factor of 1/2 comes from Re{} operation).
*   **Filtering:** Passband filter $H_p(f)$ corresponds to complex baseband filter $H(f)$. Output $Y(f) = \frac{1}{2} U(f) H(f)$.
*   **Frequency/Phase Offset:** $\tilde{u}(t) = u(t)e^{j\phi(t)}$, where $\phi(t) = 2\pi \Delta f t + \gamma$. Causes rotation/mixing of I/Q components in the receiver if uncompensated. (Fig 2.28, Eq 2.77).
![Pasted image 20250423110804.png](/img/user/College%20courses/Sem-4/CT/Pasted%20image%2020250423110804.png)
![Pasted image 20250423110737.png](/img/user/College%20courses/Sem-4/CT/Pasted%20image%2020250423110737.png)
---

# Chapter 3: Analog Communication Techniques

## Amplitude Modulation (AM) (Sec 3.2)

### DSB-SC (Double Sideband Suppressed Carrier) (Sec 3.2.1)
*   **Motivation:** Simple modulation concept; directly maps message $m(t)$ to amplitude.
*   **Generation:** $u_p(t) = A_c m(t) \cos(2\pi f_c t)$. Complex envelope $u(t) = A_c m(t)$.
*   **Spectrum:** $U_p(f) = \frac{A_c}{2} [M(f - f_c) + M(f + f_c)]$. $BW = 2B$.
*   **Demodulation:** Requires **coherent detector** (multiply by local carrier $2\cos(2\pi f_c t + \theta_r)$, then LPF).
*   **Output:** $m_{out}(t) \propto m(t) \cos\theta_r$.
*   **Challenge:** Needs accurate carrier phase synchronization ($\theta_r \approx 0$). Phase error causes signal attenuation.

### Conventional AM (Sec 3.2.2)
*   **Motivation:** Simplify receiver by enabling non-coherent **envelope detection**. Tradeoff: power efficiency for Rx simplicity (common in broadcasting).
*   **Generation:** Add a large carrier component: $u_p(t) = A_c (1 + a m_n(t)) \cos(2\pi f_c t)$.
* $a = \frac{A|m(t)_{min}|}{Ac}$
*   **Condition for Envelope Detection:** Modulation index $a \le 1$ ensures envelope $e(t) = A_c(1+am_n(t))$ follows the message shape.
*   **Envelope Detector:** Diode + RC LPF. Simple circuit.
*   **Power Efficiency:** Inefficient due to large carrier power. $\eta_{AM} = \frac{a^2 \overline{m_n^2}}{1 + a^2 \overline{m_n^2}}$.  $\eta_{AM} \leq 50 \text{ Always stays in this bound}$


###  SSB (Single Sideband) (Sec 3.2.3)
*   **Motivation:** Improve **bandwidth efficiency** over DSB by transmitting only one sideband. $BW = B$.
*   **Spectrum:** Requires filtering DSB to remove one sideband (difficult near DC) or phase shift method.
*   **Hilbert Transform:** Conceptual tool. $\hat{m}(t) = m(t) * \frac{1}{\pi t}$. Frequency domain $H(f) = -j \text{sgn}(f)$ (introduces $-90^\circ$ phase shift).
*   **Generation (Phase Shift Method):**
    $$ u_{USB/LSB}(t) = m(t) \cos(2\pi f_c t) \mp \hat{m}(t) \sin(2\pi f_c t) $$
    *   (Scaling factors often included).
*   **Complex Envelope:** $u_{USB/LSB}(t) = m(t) \pm j \hat{m}(t)$. (Note: Complex envelope is *not* just $m(t)$).
*   **Demodulation:** Requires coherent detection. Received I component is $m(t)\cos\theta_r \mp \hat{m}(t)\sin\theta_r$.
*   **Challenge:** Very sensitive to phase errors $\theta_r$, which cause not just attenuation but also **distortion** due to crosstalk from the Hilbert transform term.

###  QAM (Quadrature Amplitude Modulation) (Sec 3.2.5)
*   **Motivation:** Transmit two independent messages ($m_c(t), m_s(t)$) in the same bandwidth as DSB ($BW=2B$) by using orthogonal carriers (cos and sin). Doubles the spectral efficiency compared to DSB/AM carrying one message.
*   **Generation:** $u_p(t) = m_c(t) \cos(2\pi f_c t) - m_s(t) \sin(2\pi f_c t)$.
*   **Complex Envelope:** $u(t) = m_c(t) + j m_s(t)$.
*   **Demodulation:** Requires coherent receiver with two branches (I and Q).
*   **Challenge:** Phase errors $\phi(t)$ cause **crosstalk** between I and Q channels: $\tilde{u}_c \propto m_c\cos\phi + m_s\sin\phi$; $\tilde{u}_s \propto m_s\cos\phi - m_c\sin\phi$.

## Angle Modulation (Sec 3.3)
*   **Motivation:** Constant envelope $A_c$ makes it robust to amplitude nonlinearities (e.g., power amplifiers working near saturation). Can trade bandwidth for improved noise performance.
*   **Generation:** $u_p(t) = A_c \cos(2\pi f_c t + \theta(t))$.
*   **FM:** Instantaneous frequency $f_i(t) = f_c + k_f m(t)$. Phase $\theta(t)$ is integral of message. Smooth phase.
*   **PM:** Phase $\theta(t) = k_p m(t)$. Instantaneous frequency depends on derivative of message. Can have phase discontinuities if message is discontinuous.
*   **Bandwidth (Carson's Rule):** $BW_{FM} \approx 2(\Delta f + B) = 2B(\beta + 1)$. Requires significantly more bandwidth than AM for large $\beta$.
*   **Demodulation (FM):** Convert frequency variations to amplitude variations. E.g., Limiter-Discriminator.

## Receiver Architectures (Sec 3.4)
*   **Superheterodyne:** Mixes RF to a fixed IF for easier filtering/amplification. Requires **image rejection**.
*   **Direct Conversion (Zero-IF):** Mixes directly to baseband. Simpler, but suffers from DC offset, LO leakage.

## Phase Locked Loop (PLL) (Sec 3.5)
*   **Function:** Feedback loop to align VCO output phase/frequency with input signal phase/frequency.
*   **Uses:** Carrier synchronization (coherent demod), frequency synthesis (generating stable LO frequencies), FM demodulation.
*   **Linearized Analysis:** Provides insight into tracking (phase steps, freq steps) based on loop filter order.

---

# Chapter 4: Digital Modulation

## Signal Constellations (Sec 4.1)
*   Geometric representation of symbols in complex baseband ($b[n] = b_c[n] + j b_s[n]$).
*   M-ary constellation carries $\log_2 M$ bits per symbol.
*   **Examples:** BPSK (2-PAM), QPSK (4-PSK/4-QAM), 8-PSK, 16-QAM. (Fig 4.4).

## Bandwidth Occupancy (Sec 4.2)
*   **Linear Modulation:** $u(t) = \sum b[n] p(t - nT)$.
*   **PSD (Uncorrelated Symbols):** $ S_u(f) = \frac{\sigma_b^2}{T} |P(f)|^2 $. Bandwidth depends on the pulse shape $p(t)$.
*   **Example PSDs (Fig 4.6):** Rectangular pulse has wide spectral tails ($\text{sinc}^2$ decay); Sine pulse is better.

## Design for Bandlimited Channels (Sec 4.3)
*   **Goal:** Transmit symbols at rate $1/T$ without ISI using pulses $p(t)$ that fit within channel bandwidth.
*   **Nyquist Criterion for Zero ISI:**
    *   Time: $p(kT) = \delta[k]$. Pulse has zero crossings at multiples of symbol period $T$.
    *   Frequency: $\frac{1}{T} \sum_k P(f - k/T) = \text{constant}$. Sum of shifted spectra is flat.
*   **Sinc Pulse:** $p(t) = \text{sinc}(t/T)$. Achieves minimum theoretical BW $B_{min} = 1/(2T)$. Impractical due to slow decay (1/t).
*   **Raised Cosine (RC) Pulse:** Nyquist pulse with faster decay (1/t³). Introduces **excess bandwidth** $\alpha$.
    *   $BW = (1+\alpha) B_{min} = \frac{1+\alpha}{2T}$.
    *   Tradeoff: BW vs. pulse decay rate/robustness.
*   **Square Root Raised Cosine (SRRC):** Used at Tx ($g_{TX}$) and Rx ($g_{RX}$) such that $g_{TX}(t)*g_{RX}(t)$ is RC. $g_{RX}(t) = g_{TX}^*(-t)$ (Matched Filter).

## Bandwidth Efficiency (Sec 4.3.3)
*   Measure of data rate per unit bandwidth: $\eta_B = \frac{R_b}{B} = \frac{(\log_2 M)/T}{(1+\alpha)/(2T)} = \frac{2 \log_2 M}{1+\alpha}$ (for complex signals/2D).
*   Higher $M \implies$ higher $\eta_B$.

## Orthogonal Modulation (Sec 4.4)
*   Uses M orthogonal signals $\{s_k(t)\}$.
*   **FSK Example:** Needs frequency separation $\Delta f = 1/(2T)$ (coherent) or $\Delta f = 1/T$ (non-coherent).
*   **Bandwidth:** $BW \approx M \Delta f$. Poor bandwidth efficiency $\eta_B \approx (\log_2 M)/M$.

---

# Chapter 5: Probability and Random Processes

## Noise Modeling (Sec 5.8)
*   **AWGN:** $n(t)$ with PSD $S_n(f) = N_0/2$. Autocorrelation $R_n(\tau) = \frac{N_0}{2} \delta(\tau)$.
*   **Noise Power in BW B:** $P_N = N_0 B$.
*   **Complex Baseband Noise:** $n(t) = n_c(t) + j n_s(t)$, where $n_c, n_s$ are independent WGN processes each with PSD $N_0/2$. (Scaling adjusted for consistency). $n(t) \sim CN(0, N_0)$.

## WSS Processes & Filtering (Sec 5.7, 5.9)
*   LTI filtering preserves Gaussianity and (if input is WSS) WSS property.
*   Output PSD: $S_Y(f) = S_X(f) |H(f)|^2$.

## Correlation and Matched Filtering (Sec 5.9)
*   **Matched Filter** $h(t) = s^*(-t)$ maximizes SNR at output.
*   **Max SNR:** $SNR_{max} = \frac{|\langle s, s \rangle|^2}{E[|\langle n, s \rangle|^2]} = \frac{\|s\|^4}{(N_0/2) \|s\|^2} = \frac{2 \|s\|^2}{N_0} = \frac{2 E_s}{N_0}$.

---

# Chapter 6: Optimal Demodulation

## Hypothesis Testing Framework (Sec 6.1)
*   Model: Receive $Y$, choose hypothesis $H_i$ ($s_i$ sent).
*   Goal: Minimize Probability of Error $P_e$.
*   **MAP Rule:** Choose $i$ maximizing $\pi_i p(y|i)$. *Optimal.*
*   **ML Rule:** Choose $i$ maximizing $p(y|i)$. Optimal for equal priors.
*   **Example 6.1.1:** Hypothesis testing with exponential distributions.

## Signal Space Concepts (Sec 6.2)
*   **Key Idea:** Represent $M$ CT signals $\{s_i(t)\}$ as vectors $\{\mathbf{s}_i\}$ in an $n$-dimensional ($n \le M$) space using an orthonormal basis $\{\psi_k(t)\}$.
*   **AWGN Channel in Signal Space:**
    $$ \mathbf{Y} = \mathbf{s}_i + \mathbf{N} $$
    *   $\mathbf{Y}$: Received vector (components $Y[k]=\langle y, \psi_k \rangle$).
    *   $\mathbf{s}_i$: Transmitted signal vector.
    *   $\mathbf{N}$: Noise vector (components $N[k]=\langle n, \psi_k \rangle$ are i.i.d. $N(0, \sigma^2=N_0/2)$).
*   **Inner products, norms, distances are preserved.**

## Optimal Reception in AWGN (Sec 6.2.4)
*   **ML Rule (Vector Space):** Choose $i$ that minimizes Euclidean distance $\|\mathbf{Y} - \mathbf{s}_i\|^2$.
*   **Implementation:**
    *   **Correlator Bank:** Compute $\langle y, s_i \rangle$ for all $i$. Decision uses $\text{argmax}_i \{\langle y, s_i \rangle - \|s_i\|^2/2\}$.
    *   **Matched Filter Bank:** Filter $y(t)$ with $h_i(t) = s_i^*(-t)$ and sample. Output is equivalent to correlator.

## Performance Analysis (Sec 6.3)
*   **Q-Function:** $Q(x) = P[N(0,1) > x]$.
*   **Pairwise Error Probability (PEP):** $P(i \rightarrow j) = Q\left(\frac{\|\mathbf{s}_j - \mathbf{s}_i\|}{\sqrt{2N_0}}\right) = Q\left(\frac{d_{ij}}{\sqrt{2N_0}}\right)$.
*   **Binary Signaling:** $P_e = Q\left(\sqrt{\frac{2 \eta_p E_b}{N_0}}\right)$, with $\eta_p = d^2/(4E_b)$.
    *   Antipodal (BPSK) $\eta_p=1$ is 3dB better than Orthogonal/OOK ($\eta_p=1/2$). (Fig 6.20).
*   **M-ary Signaling Symbol Error Rate (SER):**
    *   **Union Bound:** $P_{e|i} \le \sum_{j \neq i} Q\left(\frac{d_{ij}}{\sqrt{2N_0}}\right)$.
    *   **Nearest Neighbor Approx:** $P_e \approx \bar{N}_{min} Q\left(\frac{d_{min}}{\sqrt{2N_0}}\right)$. Dominates at high SNR.
*   **Bit Error Rate (BER):** Requires considering the bit mapping (Gray coding preferred). For Gray codes, $BER \approx SER / \log_2 M$.
*   **Example QPSK vs 16QAM (Fig 6.25):** Illustrates power vs. bandwidth tradeoff. 16QAM is more bandwidth efficient ($\eta_B=4$ vs 2) but less power efficient (needs ~4dB higher $E_b/N_0$ for same SER).

## Link Budget Analysis (Sec 6.5)
*   Connects required $E_b/N_0$ (from performance analysis) to physical parameters.
*   Required Received Power: $P_{RX,dBm}(\text{min}) = (E_b/N_0)_{req,dB} + 10\log_{10} R_b - 174 + F_{dB}$.
*   **Friis Free Space Path Loss:** $P_{RX} = P_{TX} G_{TX} G_{RX} (\frac{\lambda}{4\pi R})^2$. Relates received power to transmit power, antenna gains ($G$), range ($R$), and wavelength ($\lambda$).
*   **Link Budget:** $P_{TX,dBm} = P_{RX,dBm}(\text{min}) - G_{TX,dBi} - G_{RX,dBi} + L_{pathloss,dB} + L_{margin,dB}$.

