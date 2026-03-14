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

- **Use Schmitt Triggered inputs (hysteresis) to suppress crosstalk**

    - It has two thresholds ($V_{th+}, V_{th-}$) for rising and falling edge.

    - Avoid the errors when we have noise input signals, especially when generating square waveform

![Basic Schematics of Schmitt Trigger](./images/image_23.png)

![DC Curve of Schmitt Trigger](./images/image_22.png)

# Power Delivery

## Margins in Design

### Clock Jitter

- Cycle to cycle jitter

    - 1st cycle period: 1ns; 2nd cycle period: 1.02ns; 3rd cycle period: 0.98ns 

- impacting factors: PLL + clock tree

    - PLL factors: VCO noise, Power noise, reference jitter

    - Clock tree: buffer noise, supply noise, crosstalk, routing variation

![Clock Jitter](./images/image_24.png)

### Coupling Noise

- Capacitive coupling between nets

    - especially for long wires and short spacing

- worset scenario: victim & aggressor are both switching

    - same direction, delay decreases

    - opposite direction, delay increases

![Coupling noise](./images/image_25.png)

### Temperature

- Transistors

    - $I_{sat} = \mu C_{ox}\frac{W}{2L}(V_{gs} - V_{th})^2$

    - Temperature (T) $\uparrow$ $\rightarrow$ current mobility ($\mu$) $\downarrow$, voltage threshold ($V_{th}$) $\downarrow$

    - In modern technique node, **Low Temperature will lead slower transistor**

    - -40°C could be the worst case

- Interconnects

    - $R(T) = R_0(1 + \alpha T)$

    - Temperature (T) $\uparrow$ $\rightarrow$ $R_{wire} \uparrow$ $\rightarrow$ RC delay $\uparrow$

    - 125 °C could be the worst case

![Temperature Impacts](./images/image_26.png)

### Voltage

- VRM (Voltage Regulator Module) offset

    - PMIC could have DC offset, e.g., $1.0V \rightarrow 0.98V$

- Regulator Noise and Response

    - CPU switching $\rightarrow$ Current changes suddenly $\rightarrow$ Regulator responses not immediately $\rightarrow$ VDD variates

- IR drop

    - If the current is large in some area, $V = I \times R$, voltage drops

- Inductance Drop

    - $V = L\frac{di}{dt}$

    - when multiple transistors switching $\rightarrow$ $\frac{di}{dt} \uparrow \rightarrow$ voltage drop $\uparrow$ 

![Voltage droops](./images/image_27.png)

### Process

- global corners

    - the entire wafer is fast or slow

    - FF: fast transistor, TT: typical, SS: slow transistor

- local mismatch

    - The different area of one single chip could be different

    - Clock skew, timing variation

- device + wire

    - transistor

    - metal width, metal thickness, via resistance

![Process Corners](./images/image_28.png)

### Safety Margin

- human nature

- 2nd / 3rd order effects

- "unkown unkowns"

![Safety Margin](./images/image_29.png)

![Final Margin of CLK](./images/image_30.png)

## Design Margin Taxonomy

Here are some noise factors we need to reserve margins for when we are designning the system

![Design Margin](./images/image_31.png)

- Usually, in terms of temporal rate of change, **dynamic noise is harder to handle**; in terms of spatial region, **local noise is harder to handle**

## Energy-Efficient Design: Traditional Approaches

### Industry: Other approaches prioritized till recently

- Continued sacling

    - Dennard scaling

    - Smaller technology node, smaller capacitor, samller supply voltage

- Clock gating

    - reduce flipping activity factor $s$ to reduce dynamic power

- Power gating

    - reduce leakage current and static power consumption

- DVFS (Dynamic Voltage and Frequency Scaling)

    - Dynamically control voltage and frequency

    - e.g., when CPU is idle, let voltage drops from 1.2V to 0.9V, frequency drops from 3GHz to 1.8GHz

- Multi-Vth

    - critical path, low Vth

    - non-critical path, high Vth

### Academia, focus on more exotic approaches

- Adiabatic logic / Charge-recycling techniques

- Super-pipeling (voltage-scaling)

    - Decrease logic path/delay and increase clock frequency, as a result, reduce voltage to achieve same performance

- Low-power logic topologies

    - Pseudo-NMOS logic

    ![Pseudo-NMOS example](./images/image_32.png)

    ![W/Lp ratio of Pseudo-NMOS](./images/image_33.png)

    - Pass-transistor

    ![Pass Transistor Transient Curve](./images/image_34.png)

    ![Pass Transistor AND gate](./images/image_35.png)

    - Transmission-gate

    ![Transmission Gate](./images/image_36.png)

    - Dynamic logic

    ![Dynamic Gate](./images/image_37.png)

    ![Dynamic Gate with Charge Leakage Solution](./images/image_38.png)

    - Domino logic

    ![Design with Domino Logic](./images/image_39.png)

## Voltage Margins: Challenge Perceptions

### Industry

- Traditionally recognized importance but until recently, not addressed as a design problem

- Mitigated using **binning** and **test**

    - good chip $\rightarrow$ low voltage and high frequency

    - bad chip $\rightarrow$ high voltage and low frequency

    - If timing is unstable, increase voltage and decrease frequency

    - **Remediate through operating conditions rather than fixing from design**

### Academia

- Largely unheralded

- Not seen as highly impactful effort in low-power design

### Reality

- Dominant source of inefficiency

- Voltage Margin

    - PVT variation

    - IR drop

    - Supply noise

    - Process variation

    - Temperature

- Solutions (particularly supply noise) span variety of areas

    - PDN design

        - Power Grid

        - decoupling capacitors

        - IR drop reduction
    
    - Voltage Regulation

        - VRM (Voltage Regulator Module)

        - on-chip regulator

        - LDO (Low-Dropout)
    
    - Clocking

        - adaptive clocking

        - dynamic timing margin
    
    - Micro-architecture

        - Error Recovery

            - e.g., SRAM/DRAM soft error use (Error Correcting Code(ECC)) to correctify
        
        - Razor Flip-flop

            - detect timing error (cache side effect in exception handle)

## PDN and Supply/Power Noise in Modern SoCs

### High Performance Design

When $P_{supply}$ keeps unchanged, to improve the performance ($delay \propto \frac{V_{DD}}{(V_{DD} - V_{th})^{\alpha}}$)

- $V_{DD} \rightarrow \alpha V_{DD}$ 

- $I_{DD} \rightarrow \frac{I_{DD}}{\alpha}$

- $Z_{targ} \rightarrow \frac{\alpha V_{DD}}{\frac{I_{DD}}{\alpha}} = \alpha^2 Z_{targ}$

To keeps $Z_{targ}$ as small as possible, now **the PDN has to much stronger, allowing smaller impedence than before**.

- e.g. Allow 50 $m \Omega$ in PDN, for high performance design, now only allow 10 $m \Omega$

### Low Power Design

For Low Power Design, we usually want to reduce leakage current

- $V_{th} \uparrow$ and $V_{DD} \downarrow$

- delay sensitivity to supply noise $\uparrow$

- voltage-domain count $\uparrow$

    - e.g., CPU core 0.8V, GPU 0.9V, SRAM 0.7V, IO 1.2V

- PDN becomes much complex, and noise coupling increases

**Supply noise significantly impacts performance**

In summary, Supply/Power Distribution Network (PDN), and Supply Noise (SN) become critical in Modern SoCs

## PDN Delivery Network

## Decap Parasitics

## Supply Noise


