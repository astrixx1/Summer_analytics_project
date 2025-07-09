# Summer_analytics_project

**Project Overview**

This project implements a real-time dynamic pricing system for smart parking infrastructure using streaming data. It simulates intelligent pricing strategies to manage parking demand, improve space utilization, and encourage efficient vehicle distribution. The system processes live data such as occupancy, capacity, queue length, and traffic conditions to calculate and visualize adaptive parking prices across multiple lots in real time. Two models are implemented to showcase both simple and advanced pricing logic using Python and the Pathway streaming engine.

**Tech Stacks Used**

Core Frameworks & Libraries

1)Python 3.11 – Main programming language

2)Pathway – Real-time data stream processing and temporal windowing

3)Pandas – Preprocessing CSV data and handling tabular input

4)NumPy – Numerical operations and transformations

Visualization
1)Bokeh – Interactive plotting for time-series parking price visualization

2)Panel – Displaying dynamic Bokeh plots in dashboards and Colab

Development Environment

1)Google Colab – Execution environment for notebooks with integrated plotting

**Project Architecture**

**Model 1**

![Untitled diagram _ Mermaid Chart-2025-07-09-172714](https://github.com/user-attachments/assets/ae5614ec-1293-419a-9ce2-fa92b234d5f2)

**Model 2**

![Untitled diagram _ Mermaid Chart-2025-07-09-173839](https://github.com/user-attachments/assets/b70b5652-533f-4f90-bb2e-d44b864a9fe2)

**Detailed explanation of project architecture and workflow**

Model 1 implements a simple linear pricing strategy that adjusts the parking price in real time based on how full the lot is at the moment. The model processes each row of data independently, using Occupancy and Capacity values to compute an occupancy ratio for the lot. It applies the formula:
price = base_price + alpha * (occupancy / capacity - 0.5)
where alpha is a tunable factor that determines how aggressively the price responds to crowding. When the occupancy is exactly 50%, the price equals the base (e.g., ₹10). If the lot is mostly full (e.g., 90% occupancy), the price increases; if it's mostly empty, the price decreases — but within controlled bounds. This model avoids temporal aggregation or windowing and calculates a new price for every incoming row. It is intentionally designed to be simple, interpretable, and reactive. Prices are visualized in real time using Bokeh plots served through a Panel interface, showing live updates per lot.

Model 2 takes a more granular and intelligent approach by updating the price on a per-row (i.e., per reading) basis. It uses additional features from the data such as QueueLength, TrafficConditionNearby, IsSpecialDay, and VehicleType, along with the core Occupancy and Capacity. Each record is transformed into a weighted score using a custom linear combination of these inputs. For example, higher queue lengths, special days, and heavier vehicle types (like trucks) increase the score. The score is passed through a nonlinear activation function,sigmoid to produce a normalized demand_score ∈ [0,1]. The final price is then computed as:
price = base_price * (1.0 + 0.2 * (demand_score - 0.5))
This formulation ensures prices vary dynamically around the base price, allowing both upward and downward adjustment. By controlling the output range and the slope of the activation function, we avoid extreme price spikes while still responding to demand. Each updated price is plotted in real time for the selected parking lot using a dropdown-powered Panel interface, delivering a fast and interactive experience.






