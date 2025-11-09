<h1>Voice Modulation and Demodulation</h1>

<h2>Aim:</h2>

To record a real-world voice signal and perform modulation and demodulation using
Amplitude Modulation (AM) in order to understand how analog/digital communication systems
transmit and recover information.

<h2>Theory:</h2>

Modulation is the process of varying a high-frequency carrier signal according to the amplitude,
frequency, or phase of a message signal.
1. In Amplitude Modulation (AM), the amplitude of the carrier signal is varied in
proportion to the message signal while the frequency remains constant.
2. Demodulation is the process of extracting the original message signal from the
modulated carrier.
3. Using real-world signals like voice helps in understanding practical communication
systems and observing how signals are transmitted and recovered.

<h2>Algorithm:</h2>

Step 1: Record a short voice message and save it as a .wav file.
Step 2: Import the voice signal into the software platform (Scilab).
Step 3: Normalize the voice signal to limit its amplitude.
Step 4: Define carrier parameters: amplitude and frequency.
Step 5: Perform AM modulation:
s(t)=[Ac+m(t)]⋅ cos (2πfct)s(t) = [A_c + m(t)] \cdot \cos(2 \pi f_c
t)s(t)=[Ac +m(t)]⋅ cos(2πfc t)
where AcA_cAc = carrier amplitude, fcf_cfc = carrier frequency, m(t)m(t)m(t) = message
signal.
Step 6: Plot the message, carrier, and modulated signals.
Step 7: Perform demodulation using envelope detection or filtering.
Step 8: Normalize the recovered signal and compare it with the original message.

<h2>Code:</h2>
<h2>SCILAB</h2>
```
Voice_Max_Amp = 0.5830994;
[Voice, fs_old, bits] = wavread("C:\Users\acer\Downloads\Voice.wav");
if size(Voice,1) < size(Voice,2) then
Voice = Voice';endif size(Voice, 2) > 1 then
Voice = Voice(:,1);
endfs = 21200;
t_old = (0:length(Voice)-1)' / fs_old;duration = t_old($);
t = (0:1/fs:duration-1/fs)';
Voice_resampled = interp1(t_old, Voice, t, "linear");
Voice_resampled = Voice_resampled(~isnan(Voice_resampled));
Voice = Voice_resampled;
Ac = 2.1;
fc = 2120;
mu = 0.8;
Target_Am = mu * Ac; Scale_Factor = Target_Am / Voice_Max_Amp;
Voice = Voice * Scale_Factor;carrier = Ac * cos(2 * %pi * fc * t);
s = (Ac + Voice) .* cos(2 * %pi * fc * t);
r = s .* cos(2 * %pi * fc * t);
f_2c = 2 * fc;
window_size = floor(2 * (fs / f_2c)); if window_size < 5 then
window_size = 5;
endb_lpf = ones(1, window_size)/window_size;
a_lpf = 1;
demodulated = filter(b_lpf, a_lpf, r);
demodulated = demodulated - mean(demodulated);
R = 0.99;b_dcblock = [1, -1];
a_dcblock = [1, -R];
demodulated = filter(b_dcblock, a_dcblock, demodulated);
demodulated = demodulated / max(abs(demodulated));
N = min(50000, length(Voice));
// ---------------------------------PLOTTING --------------------------------
figure(0);
clf();
subplot(3,1,1);
plot(t(1:N), Voice(1:N));
xlabel("Time (s)");
ylabel("Amplitude");
title("Message Signal (Voice, Mu=0.8, Max Amp=5.28)");
subplot(3,1,2);
plot(t(1:N), carrier(1:N));
xlabel("Time (s)");
ylabel("Amplitude");
title("Carrier Signal");
subplot(3,1,3);
plot(t(1:N), s(1:N));
xlabel("Time (s)");
ylabel("Amplitude");
title("AM Modulated Signal");
figure(1);
clf();
subplot(3,1,1);
plot(t(1:N), s(1:N));
xlabel("Time (s)");
ylabel("Amplitude");
title("Received AM Signal");
subplot(3,1,2);plot(t(1:N), r(1:N));xlabel("Time (s)");
ylabel("Amplitude");
title("Signal After Mixing with Carrier");
subplot(3,1,3);
plot(t(1:N), demodulated(1:N));
xlabel("Time (s)");
ylabel("Amplitude");
title("Recovered Message Signal (Voice Normalized)");
```

<h2>Output Waveform:</h2>

  <img width="1773" height="1002" alt="image" src="https://github.com/user-attachments/assets/4e434ea5-0e63-46c3-a015-73b864b7b9cc" />

  <img width="1767" height="990" alt="image" src="https://github.com/user-attachments/assets/dc963b94-6c8d-42a9-91d9-5c44ace411a6" />


<h2>Conclusion:</h2>

 The voice signal was successfully modulated using Amplitude Modulation and recovered
using demodulation.
 The recovered audio closely matches the original voice, demonstrating accurate
transmission of real-world signals.
 This practical experiment helped in understanding the concepts of modulation,
demodulation, and signal visualization in communication systems.
 Listening to the recovered signal confirms the effectiveness of AM and the applied
demodulation technique.
