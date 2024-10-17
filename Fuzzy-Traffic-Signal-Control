import numpy as np
import skfuzzy as fuzz
from skfuzzy import control as ctrl
import matplotlib.pyplot as plt
import os

# I create fuzzy variables
traffic_conditions = ctrl.Antecedent(np.arange(0, 101, 1),
                                     'traffic_conditions')
signal_duration = ctrl.Consequent(np.arange(0, 61, 1), 'signal_duration')

# I define fuzzy sets for traffic conditions
traffic_conditions['low'] = fuzz.trimf(traffic_conditions.universe, [0, 0, 50])
traffic_conditions['medium'] = fuzz.trimf(traffic_conditions.universe,
                                          [20, 50, 80])
traffic_conditions['high'] = fuzz.trimf(traffic_conditions.universe,
                                        [50, 100, 100])

# I define fuzzy sets for signal duration and then define the laws
signal_duration['short'] = fuzz.trimf(signal_duration.universe, [0, 0, 30])
signal_duration['medium'] = fuzz.trimf(signal_duration.universe, [20, 30, 40])
signal_duration['long'] = fuzz.trimf(signal_duration.universe, [30, 60, 60])
Condition1 = ctrl.Rule(traffic_conditions['low'], signal_duration['short'])
Condition2 = ctrl.Rule(traffic_conditions['medium'], signal_duration['medium'])
Condition3 = ctrl.Rule(traffic_conditions['high'], signal_duration['long'])

# I make a control system and then make the simulation
traffic_signal_ctrl = ctrl.ControlSystem([Condition1, Condition2, Condition3])
traffic_simulation = ctrl.ControlSystemSimulation(traffic_signal_ctrl)

# I'm making a folder for Plots
output_folder = "modified_plots"
if not os.path.exists(output_folder):
  os.makedirs(output_folder)

# I enable interactive mode
plt.ion()

while True:
  user_input = input("MOVEMENT CONDITIONS (or 'end' to exit): ")

  if user_input.lower() == 'end':
    break

  try:
    traffic_input = float(user_input)
    traffic_simulation.input['traffic_conditions'] = traffic_input
    traffic_simulation.compute()

    # Display (signal duration)
    signal_duration_output = traffic_simulation.output['signal_duration']
    print(f"Signal duration: {signal_duration_output} seconds")

    # I clean up the previous plots
    plt.clf()

    # Plot and save the new plots
    traffic_conditions.view(sim=traffic_simulation)
    plt.savefig(
        os.path.join(output_folder,
                     f"traffic_conditions_{int(traffic_input)}.png"))
    plt.clf()
    signal_duration.view(sim=traffic_simulation)
    plt.savefig(
        os.path.join(output_folder,
                     f"signal_duration_{int(traffic_input)}.png"))
    print(f"Plots SAVED IN FOLDER {output_folder}")

  except ValueError:
    print("INVALID INTRODUCTION. ENTER NUMBER OR 'end' TO EXIT.")

# I disable interactive mode and display plans
plt.ioff()
plt.show()
