Instructions for Lab \#5
================
Jonathan Gilligan
Lab: Feb. 10; Due: Feb. 24

  - [Carbon Cycle](#carbon-cycle)
      - [**Note:**](#note)
      - [Running the GEOCARB model from
        R](#running-the-geocarb-model-from-r)
      - [Chapter 8 Exercises](#chapter-8-exercises)
          - [Exercise 8.1: Weathering as a function of
            CO<sub>2</sub>](#exercise-8.1-weathering-as-a-function-of-co2)
          - [Exercise 8.2: Effect of solar intensity on steady-state
            CO<sub>2</sub>
            concentration](#exercise-8.2-effect-of-solar-intensity-on-steady-state-co2-concentration)
          - [Exercise 8.3: The role of plants (**Graduate students
            only**)](#exercise-8.3-the-role-of-plants-graduate-students-only)
      - [Exercise from Chapter 10](#exercise-from-chapter-10)
          - [Exercise 10.1: Long-term fate of fossil fuel
            CO<sub>2</sub>](#exercise-10.1-long-term-fate-of-fossil-fuel-co2)

# Carbon Cycle

For the following exercises, you will use the GEOCARB model, which
simulates the earth’s carbon cycle.

The GEOCARB model has two time periods:

  - First, it runs for 5 million years with the “Spinup” settings in
    order to bring the carbon cycle and climate into a steady state.

  - Then, at time zero, it abruptly changes the parameters to the
    “Simulation” settings and also dumps a “spike” of CO<sub>2</sub>
    into the atmosphere and runs for another 2 million years with the
    new parameters to see how the climate and carbon cycle adjust to the
    new parameters and the CO<sub>2</sub> spike.

The quantities that are graphed include:

  - pCO2  
    is the concentration of CO<sub>2</sub> in the atmosphere, in parts
    per million.
  - WeatC  
    is the rate of CO<sub>2</sub> being weathered from carbonate rocks
    and moved to the oceans.
  - BurC  
    is the rate of carbonate being converted into limestone and buried
    on the ocean floor.
  - WeatS  
    is the rate of SiO<sub>2</sub> being weathered from silicate rocks
    and moved to the oceans.
  - Degas  
    is the rate at which CO<sub>2</sub> is released to the atmosphere by
    volcanic activity
  - tCO2  
    is the total amount of CO<sub>2</sub> dissolved in the ocean, adding
    all of its forms:
    \[ \ce{\text{tco2} = [CO2] + [H2CO3] + [HCO3-] + [CO3^{2-}]}. \]
  - alk  
    is the ocean alkalinity: the total amount of acid (\(\ce{H+}\))
    necessary to neutralize the carbonate and bicarbonate in the ocean.
    The detailed definition is complicated, but to a good approximation,
    \(\ce{\text{alk} = [HCO3-] + 2 [CO3^{2-}]}\). This is not crucial
    for this lab.
  - CO3  
    is the concentration of dissolved carbonate (\(\ce{CO3^{2-}}\)) in
    the ocean, in moles per cubic meter.
  - d13Cocn  
    is the change in the fraction of the carbon-13 (\(\ce{^{13}C}\))
    isotope, relative to the more common carbon-12 (\(\ce{^{12}C}\))
    isotope, in the various forms of carbon dissolved in the ocean
    water.
  - d13Catm  
    is the change in the fraction of \(\ce{^{13}C}\), relative to
    \(\ce{^{12}C}\) in atmospheric CO<sub>2</sub>.
  - Tatm  
    is the average air temperature.
  - Tocn  
    is the average temperature of ocean water.

### **Note:**

In this lab, you will mostly look at pCO2, but in exercise 8.2, you will
also look at the weathering.

## Running the GEOCARB model from R

I have provided functions for running the GEOCARB model from R:

To run the model:

    run_geocarb(filename, co2_spike, degas_spinup, degas_sim,
    plants_spinup, plants_sim, land_area_spinup, land_area_sim,
    delta_t2x, million_years_ago, mean_latitude_continents)

You need to specify `filename` (the file to save the results in) and
`co2_spike` (the spike in CO<sub>2</sub> at time zero).

The other parameters will take default values if you don’t specify them,
but you can override those defaults by giving the parameters a value.

`degas_spinup` and `degas_sim` are the rates of CO<sub>2</sub> degassing
from volcanoes for the spinup and simulation phases, in trillions of
molecules per year.

`plants_spinup` and `plants_sim` are `TRUE/FALSE` values for whether to
include the role of plants in weathering (their roots speed up
weathering by making soil more permeable and by releasing CO<sub>2</sub>
into the soil), and `land_area` is the total area of dry land, relative
to today. The default values are: `degas` = 7.5, `plants` = `TRUE`, and
`land_area` = 1.

The geological configuration allows you to look into the distant past,
where the continents were in different locations and the sun was not as
bright as today.  
`delta_t2x` is the climate sensitivity (the amount of warming, in
degrees Celsius, that results from doubling CO<sub>2</sub>).
`million_years_ago` is how many million years ago you want year zero to
be and `mean_latitude_continents` is the mean latitude, in degrees, of
the continents (today, with most of the continents in the Northern
hemisphere, the mean latitude is 30 degrees).

After you run `run_geocarb`, you would read the data in with
`read_geocarb(filename)`. This function will return a data frame with
the columns `year`, `co2.total`, `co2.atmos`, `alkalinity.ocean`,
`delta.13C.ocean`, `delta.13C.atmos`, `carbonate.ocean`,
`carbonate.weathering`, `silicate.weathering`, `total.weathering`,
`carbon.burial`, `degassing.rate`, `temp.atmos`, and `temp.ocean`.

## Chapter 8 Exercises

### Exercise 8.1: Weathering as a function of CO<sub>2</sub>

In the steady state, the rate of weathering must balance the rate of
CO<sub>2</sub> degassing from the Earth, from volcanoes and deep-sea
vents.

Run a simulation with `co2_spike` set to zero, and set the model to
increase the degassing rate at time zero (i.e., set `degas_sim` to a
higher value than `degas_spinup`).

1)  Does an increase in CO<sub>2</sub> degassing drive atmospheric
    CO<sub>2</sub> up or down? How long does it take for CO<sub>2</sub>
    to stabilize after the degassing increases at time zero?

2)  How can you see that the model balances weathering against
    CO<sub>2</sub> degassing (**Hint:** what variables would you graph
    with `ggplot`?)

3)  Repeat this run with a range of degassing values for the simulation
    phase and make a table or a graph of the equilibrium CO<sub>2</sub>
    concentration versus the degassing rate.
    
    Does the weathering rate always balance the degassing rate when the
    CO<sub>2</sub> concentration stabilizes?

4)  Plot the weathering as a function of atmospheric CO<sub>2</sub>
    concentration, using the data from the model runs you did in part
    (c).

### Exercise 8.2: Effect of solar intensity on steady-state CO<sub>2</sub> concentration

The rate of weathering is a function of CO<sub>2</sub> concentration and
sunlight, and increases when either of those variables increases. The
sun used to be less intense than it is today.

Run GEOCARB with the spike set to zero, with the default values of 7.5
for both `degas_spinup` and `degas_sim`, and with the clock turned back
500 million years to when the sun was cooler than today.

What do you get for the steady state CO<sub>2</sub>? How does this
compare to what you get when you run GEOCARB for today’s solar
intensity? Explain why.

### Exercise 8.3: The role of plants (**Graduate students only**)

The roots of plants accelerate weathering by two processes: First, as
they grow, they open up the soil, making it more permeable to air and
water. Second, the roots pump CO<sub>2</sub> down into the soil.

Run a simulation with no CO<sub>2</sub> spike at the transition and with
no plants in the spinup, but with plants present in the simulation.

1)  What happens to the rate of weathering when plants are introduced in
    year zero? Does it go up or down right after the transition? WHat
    happens later on?

2)  What happens to atmospheric CO<sub>2</sub>, and why?

3)  When the CO<sub>2</sub> concentration changes, where does the carbon
    go?

## Exercise from Chapter 10

### Exercise 10.1: Long-term fate of fossil fuel CO<sub>2</sub>

Use the GEOCARB model in its default configuration.

1)  Run the model with no CO<sub>2</sub> spike at the transition. What
    happens to the weathering rates (Silicate, Carbonate, and Total) at
    the transition from spinup to simulation (i.e., year zero)?

2)  Now set the CO<sub>2</sub> spike at the transition to 1000 GTon.
    
      - What happens to the weathering at the transition? How does
        weathering change over time after the transition?
    
      - How long does it take for CO<sub>2</sub> to roughly stabilize
        (stop changing)?

3)  In the experiment from (b), how do the rates of total weathering and
    carbonate burial change over time?
    
      - Plot what happens from shortly before the transition until
        10,000 years afterward (**Hint:** you may want to add the
        following to your `ggplot` command: `xlim(NA,1E4)` to limit the
        range of the *x*-axis, or `scale_x_continuous(limits =
        c(NA,1E4), labels = comma))` if you also want to format the
        numbers on the *x*-axis with commas to indicate thousands and
        millions.)
        
        How do the two rates change? What do you think is happening to
        cause this?
    
      - Now plot the carbon burial and total weathering for the range 1
        million years to 2 million years. How do the two rates compare?
