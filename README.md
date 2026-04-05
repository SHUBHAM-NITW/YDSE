
<img width="2816" height="1536" alt="Gemini_Generated_Image_le3nmyle3nmyle3n" src="https://github.com/user-attachments/assets/fc83acac-08d4-41a7-82bf-17b215700be6" />

# 🌊 Young's Double Slit Experiment (YDSE)

Welcome! This repository explores the fundamental principles of wave optics and interference through the classic Young's Double Slit Experiment. It includes theoretical foundations, visual guides, and a complete Finite Difference Time Domain (FDTD) simulation setup using Ansys Lumerical.

## 📖 What is YDSE?
First performed by Thomas Young in 1801, this experiment demonstrates the wave nature of light. When a coherent light source passes through two closely spaced parallel slits, the waves emerging from the slits overlap and interfere with each other. 

This interference creates a distinct pattern of alternating bright (constructive interference) and dark (destructive interference) fringes on a viewing screen placed behind the slits.

### 📐 Core Governing Equations
The geometry of the interference pattern is predicted by two primary formulas:

* **Fringe Width (β):** The distance between two consecutive bright or dark fringes.
  > **β = (λ * D) / d**
* **Fringe Position (Yₙ):** The exact distance of the *nth* bright fringe from the central maximum.
  > **Yₙ = (n * λ * D) / d**

*(Where **λ** = Wavelength, **D** = Distance to screen, **d** = Slit separation, and **n** = Fringe order).*

---

## 📊 Comprehensive Visual Guide
*(A visual cheat sheet detailing how changing wavelength, screen distance, and slit separation manipulates the interference pattern).*

![Comprehensive YDSE Guide](./path/to/your/ydse_poster.png)

---

## 🖥️ Lumerical FDTD Simulation
To visualize the electromagnetic wave propagation in real-time, this project utilizes a 2D FDTD simulation. The screen is modeled using a Perfect Electrical Conductor (PEC) to ensure a perfectly opaque barrier, and a Continuous Wave (CW) plane source is injected to generate a sustained, bright interference pattern.

### Simulation Script (LSF)
You can build the entire setup instantly by running the following script in the Lumerical Script Prompt:
<img width="1697" height="920" alt="04-04-26-175811" src="https://github.com/user-attachments/assets/534d9a26-fe5a-40f2-a46c-86629d645b8a" />

<img width="1021" height="479" alt="04-05-26-175909" src="https://github.com/user-attachments/assets/2c3c1399-b20e-404f-8fbe-d0a2f936f35a" />
<video src="./ydse1_ydse_movie.mp4" width="800" controls autoplay loop>
  Your browser does not support the video tag.
</video>
https://github.com/user-attachments/assets/d3e62e4a-1386-4ea6-80ef-eb33e3e418ce


```lumerical
# ==========================================
# YDSE Continuous Wave Simulation Setup
# ==========================================

clear; newproject;

# 1. Define Parameters
wl = 0.5e-6;             # Wavelength (500 nm)
slit_width = 0.5e-6;     # Width of each slit
slit_distance = 3e-6;    # Distance between slit centers
screen_thickness = 0.1e-6; 
sim_span_x = 12e-6;      
sim_span_y = 15e-6;      

# 2. Add FDTD Region
addfdtd;
set("dimension", "2D");
set("x", 0); set("x span", sim_span_x);
set("y", 0); set("y span", sim_span_y);
set("mesh accuracy", 2);
set("simulation time", 2000e-15); # Extended time for steady CW pattern

# 3. Build PEC Screen Blocks
addrect; set("name", "middle_block"); set("x", -sim_span_x/4); set("y", 0); set("x span", screen_thickness); set("y span", slit_distance - slit_width); set("material", "PEC (Perfect Electrical Conductor)");
addrect; set("name", "top_block"); set("x", -sim_span_x/4); set("y", sim_span_y/2); set("x span", screen_thickness); set("y span", sim_span_y - (slit_distance + slit_width)); set("material", "PEC (Perfect Electrical Conductor)");
addrect; set("name", "bottom_block"); set("x", -sim_span_x/4); set("y", -sim_span_y/2); set("x span", screen_thickness); set("y span", sim_span_y - (slit_distance + slit_width)); set("material", "PEC (Perfect Electrical Conductor)");

# 4. Add Continuous Wave (CW) Source
addplane;
set("injection axis", "x-axis"); set("direction", "Forward");
set("x", -sim_span_x/4 - 1e-6); set("y", 0); set("y span", sim_span_y);
set("center wavelength", wl);
set("pulse type", "Continuous"); # Set to CW for bright, sustained pattern

# 5. Add Monitors
addprofile; set("name", "interference_profile"); set("x", sim_span_x/8); set("x span", 3*sim_span_x/4); set("y", 0); set("y span", sim_span_y);

addmovie; set("name", "ydse_movie"); set("x", sim_span_x/8); set("x span", 3*sim_span_x/4); set("y", 0); set("y span", sim_span_y);
set("scale", 5); # Boost visual intensity

zoomextent;
