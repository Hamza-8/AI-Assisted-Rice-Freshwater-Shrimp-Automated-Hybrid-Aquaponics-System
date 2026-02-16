# AI-Assisted-Rice-Freshwater-Shrimp-Automated-Hybrid-Aquaponics-System
A smart aquaponics system optimized for Egypt's environment and rice and fish species.

**1 Project Rationale**
**1.1 Background**
**Rice cultivation in Egypt**
Rice is a major stable crop in Egypt and is primarily cultivated in the northern Nile Delta region. Key rice-producing governorates include Kafr El Sheikh, Beheira, Damietta, and Sharqia, where the water availability and soil conditions allow for large-scale production (SJP Tours, n.d.).

The primary challenge with rice cultivation is its large water demand, considering Egypt’s water scarcity. Most farmers rely on the traditional flooding method for rice, where the fields are submerged for almost the whole season (SJP Tours, n.d.). This puts pressure on the limited water supply and sparks competition between sectors (agriculture, industry, households, etc.). The Egyptian government has been implementing policies to reduce rice cultivation areas and boost water-saving practices; however, managing food and water security remains an issue.

In Hussein's (2021) assessment of water usage for rice irrigation across the nation, they report an average consumption of 6563 m3 per feddan, and estimate that rice cultivation consumed 7.9 billion m3 of water in 2020, roughly a third of total summer crop usage. Hussein (2021) also found that shifting to modern short and medium-duration rice varieties greatly reduces water consumption.

**Water-saving rice irrigation method: Alternative Wetting and Drying (AWD)**
Alternative Wetting and Drying (AWD) is a modern method for controlled rice irrigation designed to manage water usage while maintaining yields. It involves the flooding of fields to depths of about 3-5cm, then letting them dry until the water level reaches about 10-15cm below the soil surface, after which the field is reflooded (Mote et al.,2020).

Despite the variance of AWD performance by soil and climate, it’s safe to say that it results in nontrivial water usage savings. For example, Mote et al. (2020) reported irrigation water saving of 26.6-35% compared to continuous flooding (Telangana, India). It’s worth noting that AWD guidance lists a range of 1-7 days as the drying period, and that ponding should be maintained during flowering periods to avoid yield loss.

**Aquaponics as a water-saving and nutrient-recycling technique**
Hybrid quaponics systems are an integrated loop where water is shared between fish production and crop cultivation. It’s described as a method that can increase fish and crop production per unit area and water, requiring careful water quality management (El-Essawy et al., 2019).

Several aquaponics systems have been applied and validated in Egypt. For example, aquaponics systems combining tilapia with leafy crops and herbs have been implemented in Sohag (Soliman et al., 2022; Soliman et al., 2023). Moreover, aquaponics system have been tested in desert conditions, monitoring pH, oxygen, and
nitrogen compounds (El-Gabry & Jaskolski, 2021). Most Egyptian studies focus on tilapia with low-nutrient-requirement, low-strategic-value vegetables and herbs. This creates an opportunity to test a rice-focused aquaponics system that implements AWD, nutrient reuse using real-time sensing, automated actuation, while producing clear evidence.

**1.2 Problem Statement**
Rice cultivation in Egypt’s Nile Delta relies heavily on traditional continuous flood irrigation, making it one of the most water-demanding crops in a country facing increasing water scarcity. Existing cultivation practices waste water, while farmers face growing pressure to adopt water-saving approaches without sacrificing yield or productivity.
This project targets the problem of water waste and irrigation inefficiency in the Delta-region rice cultivation by
testing a small-scale prototype that applies AWD-style irrigation and aquaponics nutrient reuse using real-time sensing, automated actuation, and data-driven monitoring.
**Project Objectives**
1. Build a compact hybrid aquaponics prototype (U-shaped 10L fish tank + 0.1 m2 rice paddy) suitable for
outdoor testing in an Egypt-relevant context.
2. Implement real-time monitoring and closed-loop control of four environmental parameters: pH, turbidity
(proxy via visibility), light intensity (lux), and flood depth (water level).
3. Implement AWD-style irrigation control for rice by:
1• flooding to a controlled depth of 5cm, and
• ending the dry phase using a soil-moisture trigger (re-flooding decision).
This aligns with IRRI AWD guidance that reflooding can be done to about 3–5 cm and re-irrigation is triggered when water drops 10-15cm below the soil surface (International Rice Research Institute, n.d.-b)
4. Maintain system pH in a target band of 6.8–7.2 using microdosing (KOH / HCl) and quantify stability (timein-range, number of dosing events, overshoot).
5. Use AI on the Arduino Uno Q to (1) detect anomalies via computer vision and (2) run a regression model
that predicts yield-related indicators and recommends parameter adjustments.
6. Quantitatively compare performance using two 5-day outdoor trials: a manual baseline vs. full automation,
with consistent logging and a defined analysis plan.
7. Deliver a lightweight Flutter app (Firebase backend) for live dashboarding and database-driven parameter
profiles (preloaded + editable).

**2 System Design
2.1 Physical Structure**
Tank and paddy geometry
The system is designed as:
• A U-shaped fish tank with a capacity of 10 L,
• Topped by a 0.1 m2 rice paddy bed with 15 cm depth,
• Outer footprint 40 cm × 25 cm, with 10 cm channel width for the U-shaped tank,
• The inner section of the U-shaped tank is reserved for electronics and wiring isolation.

**MCU/compute platform: Arduino Uno Q**
The controller is the Arduino Uno Q, which combines a Linux-capable application processor with a real-time MCU subsystem and supports hybrid development workflows that include Python and Arduino sketches (Arduino, 2026). This makes it suitable for (1) sensor/actuator real-time control and (2) running lightweight edge AI inference.

**2.2 Automation & AI
pH**
This project targets pH = 6.8–7.2 as a practical compromise among rice needs, nitrifier activity, and freshwater prawn aquaculture constraints:
• Flooded rice soils: flooding tends to equalize soil pH toward near-neutral values (around pH 7), improving nutrient availability (International Rice Research Institute, n.d.-a; Roger & Heong, 1996).
• Macrobrachium rosenbergii: growth is reported with optimal pH around 7.0–8.5 in culture guidance
summaries (U.S. Fish and Wildlife Service, n.d.), and prawn culture notes also list a preferred alkaline
range (often 7.5–8.5) (Ramesh, 2001).
• Nitrifiers (Nitrosomonas / Nitrobacter): nitrification performance is strongly pH-dependent; studies
modeling nitrifier growth report an optimum around pH ≈ 7.8 (Antoniou et al., 1990), while a Nitrobacter study reports relatively stable activity in pH 7–9 and strong inhibition at pH 6 (Ma et al.,
2014).
Given the different optima and the desire to avoid high-pH plant nutrient lockout, this system will use 6.8–7.2
as a balanced band.

A pH probe measures water pH continuously. If pH drifts out of range:
• KOH is dosed via a stepper-driven syringe pump to increase pH,
• HCl is dosed via a second stepper-driven syringe pump to decrease pH.

**Turbidity**
Turbidity is treated as a controllable water-clarity proxy. In freshwater prawn pond culture, a commonly cited operational guideline is to maintain water transparency around 30-40cm Secchi-disk visibility (Ramesh, 2001). In this prototype, turbidity will be controlled by adjusting the aquarium filter duty cycle (timed on/off), aiming to keep visibility in a calibration-defined range (calibrated by jar test, see below).

**Light Intensity**
A lux sensor measures incident light. A PWM-dimmable LED provides supplemental lighting on cloudy days, targeting the system’s measured baseline for a normal sunny day (local calibration). The controller only supplements during daylight hours (RTC-gated) to avoid disturbing circadian patterns.

**AWD**
Flood depth (water level) is controlled using a water level sensor and a pump/valve actuation:
• Flood phase: fill until the measured water depth reaches 5 cm.
• Dry phase: stop flooding and allow drawdown/uptake; re-flood when soil moisture reaches the selected
threshold.
The 5 cm setpoint aligns with AWD guidance that reflooding can be done to about 3–5 cm (International
Rice Research Institute, n.d.-b).

**Computer Vision**
The anomaly detection model will flag visible issues (e.g., abnormal discoloration, leaf rolling, mold blooms). The vision pipeline will compute:
• Canopy cover (%): segmentation-based estimate of plant area/frame area.
• Greenness index: vegetation index features appropriate for RGB cameras.
• Other candidate features: leaf color shift statistics (chlorosis proxy), texture/spot detection for disease-like patterns, and growth rate slope over time (derived feature).

**Warnings and Status Display**
A small OLED provides live status summaries, while traffic-light LEDs indicate:
• green: all parameters within target ranges,
• yellow: warning (approaching bounds / minor anomaly),
• red: critical warning (out-of-range / detected anomaly / actuator error).

**Sensor Calibration Plan**
• pH sensor calibration: two-point or three-point calibration using distilled water rinses and cross-checking against a lab-grade pH meter. Store slope/offset for conversion.
• Turbidity sensor calibration: jar test calibration to the operational clarity band (Secchi stick test). Establish turbidity sensor readings that correspond to target visibility thresholds based on controlled samples.
• Lux sensor calibration: record lux values on a normal sunny day to define the local baseline range and use this as the target range during the day (supplementing on cloudy days).
• Water level sensor calibration: map raw readings to known water depths (e.g., 0 cm, 2 cm,
5 cm, 10 cm) using a ruler-measured reference and fit a linear conversion.

**2.3 Web/Mobile App**
A Flutter app (Firebase backend) will include:
(a) Dashboard screen: live sensor readings, parameter status, actuator states, and warnings.
(b) Database screen: parameter-profile databases containing pH, turbidity/visibility, lux target bands, and flood-depth/AWD rules for crop + aquatic species.

Two databases will be preloaded (Rice + freshwater prawn and a second fish/crop pair suitable for Egypt), with full functionality (create, edit, delete).

**3 Experimental Design
3.1 Outdoor Trials**
This project will run two outdoor trials under similar environmental conditions:
• Trial A (Manual baseline, 5 days): sensing + logging enabled, control actions performed manually using live readings.
• Trial B (Full system, 5 days): closed-loop control enabled for AWD flooding, pH microdosing, LED supplementation, and turbidity control via filtration duty, some regression model recommendations implemented.

**Logging Plan**
Using an RTC, the Arduino Uno Q will log one CSV sheet per day per trial containing: timestamp, pH,
turbidity/visibility proxy, lux, flood depth, soil moisture, actuator states (LED PWM, filter duty, dosing
steps), and AI outputs (canopy cover %, greenness index, anomaly flags).

**Comparison Metrics**
• Control stability: time-in-range for each parameter; out-of-range count and duration; number of
corrective events.
• Water-use proxy: daily water added to maintain AWD cycling and losses.
• Growth indicators: daily canopy cover change, greenness change, anomaly frequency/timing.

**3.2 Data Analysis**
**MATLAB Time-Series Analysis**
MATLAB will be used to produce time-series plots for each parameter across both trials.

**Daily Growth-Rate Comparison**
Vision-derived canopy cover and greenness will be summarized daily, and a day-by-day growth-rate comparative meta-analysis will be conducted between manual vs automated operation.

**References**
Antoniou, P., Hamilton, J., Koopman, B., Jain, R., Holloway, B., Lyberatos, G., & Svoronos, S. A. (1990).
Effect of temperature and pH on the effective maximum specific growth rate of nitrifying bacteria.
Water Research, 24(1), 97–101. https://doi.org/10.1016/0043-1354(90)90070-M
Arduino. (2026). Uno q. Arduino Documentation. Retrieved February 13, 2026, from https://docs.arduino.
cc/hardware/uno-q/

El-Essawy, H., Nasr, P., & Sewilam, H. (2019). Aquaponics: A sustainable alternative to conventional agriculture in egypt – a pilot scale investigation. Environmental Science and Pollution Research, 26,
15872–15883. https://doi.org/10.1007/s11356-019-04919-8

El-Gabry, L., & Jaskolski, M. S. (2021). Implementation of sustainable integrated aquaculture, aquaponic,
and hydroponic systems for egypt’s western desert through global community engaged research
[Paper ID #32909]. Proceedings of the 2021 ASEE Annual Conference & Exposition.

Hussein, M. A. E. H. T. (2021). Productive and water merit of the most important varieties of rice in the
arab republic of egypt. Alexandria Scientific Exchange Journal, 42(3), 1605–1618. https://doi.org/
10.21608/asejaiqjsae.2021.188821

International Rice Research Institute. (n.d.-a). Problem environments. IRRI Knowledge Bank. Retrieved
February 13, 2026, from https://www.knowledgebank.irri.org/ericeproduction/0.4_Problem_
environments.htm

International Rice Research Institute. (n.d.-b). Saving water with alternate wetting and drying (awd). IRRI
Knowledge Bank. Retrieved February 13, 2026, from https://knowledgebank.irri.org/training/factsheets/water-management/saving-water-alternate-wetting-drying-awd

Ma, S., Zhang, D., Zhang, W., & Wang, Y. (2014). Ammonia stimulates growth and nitrite-oxidizing activity
of Nitrobacter winogradskyi. Biotechnology and Biotechnological Equipment, 28(1), 27–32. https:
//doi.org/10.1080/13102818.2014.901679

Mote, K., Rao, V. P., & Anitha, V. (2020). Alternate wetting and drying irrigation technology in rice. Indian
Farming, 70(4), 6–9.

Ramesh, G. (2001). Tips for successful freshwater prawn culture. SEAFDEC Asian Aquaculture, 23(1–2),
14. Retrieved February 13, 2026, from https://repository.seafdec.org.ph/bitstream/handle/10862/
2713/RameshG2001-tips-for-successful-freshwater-prawn-culture.pdf?sequence=1

Roger, P. A., & Heong, K. L. (Eds.). (1996). Biology and management of the floodwater ecosystem in ricefields. International Rice Research Institute. Retrieved February 13, 2026, from https://books.irri.org/971220068X_content.pdf

SJP Tours. (n.d.). Rice cultivation in egypt: A comprehensive insight (Online report). SJP Tours. Retrieved
February 13, 2026, from https://sjptours.com/rice-cultivation-in-egypt-a-comprehensive-insight/

Soliman, H. A. M., Farrag, M. M., Badrey, A. E. A., Khedr, E. E.-D. A., & Kloas, W. (2022). Growth
performance and hematological, biochemical, and histological characters of the nile tilapia (Oreochromis niloticus, l.) cultivated in an aquaponic system with green onion: The first study about the aquaponic system in sohag, egypt. Egyptian Journal of Aquatic Biology & Fisheries, 26(2), 119–131.

Soliman, H. A. M., Osman, A., Abbass, M., & Badrey, A. (2023). Paired production of the nile tilapia (Oreochromis niloticus) and lettuce (Lactuca sativa) within an aquaponics system in sohag governorate. Sohag Journal of Sciences, 8(1), 35–40. https://doi.org/10.21608/sjsci.2022.165767.1037

U.S. Fish and Wildlife Service. (n.d.). Ecological risk screening summary: Giant river prawn (Macrobrachium rosenbergii) (tech. rep.). U.S. Fish and Wildlife Service. Retrieved February 13, 2026,
from https://www.fws.gov/sites/default/files/documents/Ecological-Risk-Screening-SummaryGiant-River-Prawn.pdf
