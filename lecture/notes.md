# Analyzing the DC Transfer Function

- Assume $I_{dsat,n} = I_{dsat,p}$

- $V_{th,n}$ = $V_{th,p}$ = 200mV

- Linear region $(V_{gs} > V_{th}; V_{ds} < (V_{gs} - V_{th}))$:

    - $I_{ds} = \mu C_{ox} \frac{W}{2L}[2(V_{gs} - V_{th})V_{ds} - V_{ds}^2]$

- Saturation region ($V_{ds} < (V_{gs} - V_{th}$):

    - $I_{ds} = \mu C_{ox} \frac{W}{2L}[(V_{gs} - V_{th})^2]$

- $C_{ox} = \epsilon_o\epsilon_{ox}/t_{ox}$

![Baisc Invertor's VTC](./images/image_4.png)

- Assume load is capacitive (No steady-state current draw)

- Voltages are dc (Amount of capactive load is irrelevant)

## Inverter DC analysis

- Devices operate over a variety of modes across voltage range

- Governing equation: **Pullup current == Pulldown current**

    - $I_{NEFT}(V_{gsn}, V_{dsn}, V_{thn}) = I_{PEFT}(V_{gsp}, V_{dsp}, V_{thp})$

    - where $V_{gsn} = V_{in}$, $V_{dsn} = V_{out}$; $|V_{gsp}| = V_{DD} - V_{in}$, $|V_{dsp}| = V_{DD} - V_{out}$

- Both equation-based and graphical analysis is valuable

- Useful in determining robustness to noise (Static Noise Margin/Holding Res)

![CMOS Inverter VTC](./images/image_5.png)

## Further Discussions:

### 1. If both Vthp and Vthn increase to 0.8V, how does the curve look like?

![Inreased Vthp and Vthn to same value](./images/image_6.png)

- More flat

- Same Vm

### 2. If $I_{dsat,p} = 2I_{dst,n}$, how does this curve look like?

![I_dsat,p is two folder of I_dast,n](./images/image_7.png)

- Stronger PMOS

### 3. If $|V_{thp}| > |V_{thn}|$, how does this curve look like?

![|Vthp| > |Vthn|](./images/image_8.png)

- Weaker PMOS

In short, **Stronger PFET** => **Difficult high-to-low transition** => **Higher Switching threshold (Vm)**

**Weaker PFET** => **Difficult low-to-high transition** => **Lower Switching threshold (Vm)**

![Design of Switching Threshold (Vm)](./images/image_9.png)

- Proper choice of $\beta_{n}/\beta_{p}$ is necessary to achieve a desired switching threshold (Vm)

### 4. If VDD = 1V rather 2.5V (|Vthp| = |Vthn| = 0.4V), how does the curve look like?

![VDD has smaller sweep range](./images/image_10.png)

- $gain = \frac{\delta V_{out}}{\delta V_{in}}$

- **Noise margin decreases**!

![Effect of VDD on VTC](./images/image_11.png)

- Once VDD < Vth, the Inverter now becomes a **Subthreshold Inverter**

VTC is obtained from:

$$I_{subthP} = I_{subthN}$$

$$I_{0P}e^{\frac{VDD - V_{in} - |V_{thp}|}{n_pKT/q}} = I_{0N}e^{\frac{V_{in} - V_{thn}}{n_nKT/q}}(1 - e^{\frac{qVout}{kT}})$$

we know that, $$I_{D} = I_{0}e^{\frac{q(Vgs - Vth)}{nKT}}(1 - e^{-\frac{qV_{DS}}{KT}})$$

![Sub-Threshold Conduction](./images/image_12.png)

- A pratical definition of $V_{th}$: $V_{gs}$ necessary to obtain a pre-defined current ($I_{th}$)

- A commonly used value: $I_{th} = 300nA \times (W/L)$

**Sub-threshold Slope**

Subthreshold Slope (S) is defined as the $\Delta V_{gs}$ required for 1 decade change in $I_{D}$

$$\frac{I_{D2}}{I_{D1}} = e^{\frac{q(V_{gs2} - V_{gs1})}{nKT}} = e^{\frac{q(\Delta Vgs)}{nKT}}$$

$$\Delta V_{gs} = \frac{nKT}{q}ln{\frac{I_{D2}}{I_{D1}}}$$

$$S = \frac{nKT}{q}ln(10) = \frac{kT}{q} ln(10) (1 + \frac{C_{D}}{C_{ox}})$$

Min $S = \frac{kT}{q}ln(10)$ ~60mV / decade (for n = 1) at room temp (T = 300K)

### 5. Sketch DC curve for circuit to this 'fake inverter' (Trick question)

![DC curve of 'fake inverter'](./images/image_13.png)

## Inverter Static Noise Margin

![Noise on the Circuit](./images/image_14.png)

![Special about unity gradient points](./images/image_15.png)

$$NM_L = V_{IL} - V_{OL}$$

$$NM_H = V_{OH} - V_{IH}$$

![Noise Margin NMH and NML](./images/image_16.png)

Best possible NM for an inverter:

- when the inveter is ideal, $V_{OH} = V_{DD}$, and $V_{OL} = 0$, and $V_M = VDD/2$

- the unity-gain points are symmetric, $V_{IL} \approx V_{DD}/2$ and $V_{OL} \approx V_{DD}/2$ 

- $NM_{L} = NM_{H} = V_{DD}/2$

### Logic Level Restoration in Cascaded Inverters

- **Restoration**: The process of "cleaning up" a signal so that it **returns to its full, intended voltages levels** after **its has been degraded**.

![Cascaded Inveters](./images/image_17.png)

- Unity-gain points definte the Noise Margin (NM_L and NM_H)

- If noise voltage doesn't exceed noise margin, then it can restore to full swing after noise is applied.

- Once noise voltage exceeds the noise margin, then the next inverter could get 'erroneous logic' and case **digital error propogation**

# Crosstalk

## Crosstalk Model

![Crosstalk Prototype](./images/image_18.png)

When *aggressor* switches, especially in a large slew rate, the charge will charge *victim* through **coupling capacitor**.

- $I_{coupling} = C_c\frac{dV_{agg}}{dt}$

- if aggressor has a very steep slew rate, $\frac{dV_{agg}}{dt} \neq 0$, the current will go thourgh victim node through coupling capacitor $C_c$.

### Simple Crosstalk Model

![Simple Crosstalk Model](./images/image_19.png)

### Complex Crosstalk Model

![Complex Crosstalk Model](./images/image_20.png)

### Crosstalk Model with R_Hold

![Crosstalk Model with R_Hold](./images/image_21.png)

## Crosstalk Glitch

### Key glitch parameters

- Peak glitch amplitude $V_{peak}$

    - **The most important parameter**, because if it exceeds the receiving gate threshold, it can **false switching**

- Glitch width (duration) $T_{glitch}$

    - The time the glitch stays above certain threshold. Even if the peak is high, if the pulse is **very narrow**, the next gate might **filter it out**

### Relevant glitch-determing parameters

- Coupling/Parasitic capacitance $C_c$

    - $V_{glitch} \approx C_c$

    - Factors increasing $C_c \propto \epsilon \frac{A}{d}$ and $X_c \propto \frac{1}{2\pi fc}$

        - long parallel wires

        - small spacing

        - high clock frequency (for clock signal)

        - same metal layer (long parallel wires and samll spacing, the wires at different layer are orthogonal)

- Victim node capacitance $C_g$

    - Total Capacitance on the victim net: $C_v = C_{wire} + C_{load}$

    - Larger $C_g$ can help decrease $\Delta V_v$, vice versa

- Aggressor transistion time

    - $i = C\frac{dV}{dt}$, Fast aggressor edges $\rightarrow$ large $\frac{dV}{dt}$ $\rightarrow$ larger injected current

    - fast edge $\rightarrow$ larger glitch, slow edge $\rightarrow$ smaller glitch

- Static Inverter's **holding resistance**

    - When victim node is charged by aggressor, victim driver will try to get the value back to the original one, which is called **driver strength**, and is related to **holding resistance**

    - **Holding Resistance**: The equivalent resistance of the victim driver when maintaining the current logic value

        - When victim inverter originally is *logic 0* (PMOS off, NMOS on), then the driver strength depends on the *NMOS equivalent resistance*

        - When victim inverter originally is *logic 1* (NMOS off, PMOS on), then the driver strength depends on the *PMOS equivalent resistance*

    - Why *static*? Becuase this is a general / unverisal case and is independent of event

    - The approximate value of $R_{hold} \approx \frac{1}{gm}$
    
    - **Holding resistance impacts peak noise glitch**

        $$V_{glitch} \approx I_{coupling} \times R_{hold}$$

    - If driver is weak, then $R_{hold} \uparrow \Rightarrow V_{glitch} \uparrow$ and vice versa.

- Relative switching direction

    - If victim is switching simultaneously:

        - same direction $\rightarrow$ delay decreases

        - opposite direction $\rightarrow$ worst delay

## PVT effects of X-talk

### Voltage Effect

For the model with $R_{hold}$

- $V_{DD}\downarrow \rightarrow I_{coupling} \downarrow \rightarrow$ slew rate $\downarrow$

- But victim has **lower noise margins**

- If $V_{DD} \downarrow$, victim will has **higher** $R_{hold}$ $\rightarrow$ glitch peak $\uparrow$

So if only $V_{DD} \downarrow$, it hard to tell crosstalk could be better or worse.

- It usually needs to simulate through SI analysis tool

- **But in modern technology, the result of $V_{DD} \downarrow$, worse noise margin, weaker driver, higher $R_{hold}$, and glitch peak $\uparrow$ dominates**

### Something important about Packaging

- **Coupling is not restricted to on-chip -> impacts off-chip operation as well**

    - PCB / Cable / Board

- **Long runs of closely spaced wires with no ground-plane/coupling around are worst**

    - Clock buffer tree / Segement into blocks

    - **Shielding**: add a protect VSS wire between two long / high frequency wires

- **Use Schmitt Triggered inputs (hystersis) to suppress crosstalk**

    - It has two thresholds ($V_{th+}, V_{th-}$)

    - Minor noise will not repeatly trigger

![DC Curve of Schmitt Trigger](./images/image_22.png)

