Digital City: Project Design Document
1. Project Overview
The "Digital City" project will create a large-scale, agent-based model of the economy of Birmingham. The simulation will be populated by autonomous agents representing citizens and businesses. Each agent will make decisions based on a set of rules, and their collective interactions will produce emergent, city-wide economic and social patterns. The primary goal is to build a tool for exploring how individual behaviors can lead to complex phenomena like gentrification, economic growth, and traffic congestion.

2. Core Concepts
Agent-Based Modeling (ABM): The simulation is built from the bottom up. We define the behavior of individual "agents" (citizens, businesses) and the environment they inhabit. Macro-level patterns are not programmed directly but emerge from the agents' interactions.

The Environment: The simulation world is a digital representation of Birmingham. Initially, this will be an abstract grid, but it will later incorporate real geographic data (e.g., land value, zoning, road networks) using GeoPandas.

The Scheduler: A scheduler dictates the order in which agents act during each time step (e.g., one day). This ensures a logical flow of events, such as businesses hiring before citizens can earn money and spend it.

3. Agent Design
Citizen Agent
Represents an individual person living in the city.

Attributes:

location: Current coordinates (x, y).

wealth: Amount of money.

employment_status: Employed or unemployed.

employer: A reference to their Business agent employer.

home: Location of their residence.

Behaviors (Methods):

find_work(): Search for a Business agent that is hiring.

commute(): Move between home and work locations.

shop(): Spend wealth at a Business agent to meet needs.

move_home(): Find a new place to live based on factors like wealth and proximity to work.

Business Agent
Represents a commercial entity operating in the city.

Attributes:

location: Business coordinates (x, y).

capital: Amount of money.

employees: A list of Citizen agents it employs.

hiring: Boolean indicating if it has open positions.

industry: The type of goods or services it provides.

Behaviors (Methods):

hire(): Add a Citizen agent to its list of employees.

produce(): Generate goods/services, creating value.

pay_employees(): Transfer wealth to its Citizen agent employees.

4. Simulation Flow (Per Step)
Each step of the simulation will follow a logical sequence managed by the Mesa scheduler.

Business Actions: Businesses produce goods, set prices, and advertise open positions.

Citizen Job Market: Unemployed Citizen agents search for and accept jobs.

Work & Pay: Employed Citizen agents "work" (transferring value to the business), and businesses pay them their salaries.

Consumption: Citizen agents spend their wealth at Business agents.

State Update: Agents update their internal states (e.g., wealth, location) based on the step's events.

Data Collection: The model records key metrics (e.g., unemployment rate, average wealth, business profits).

5. Technology Strategy
This project uses a two-phase, hybrid approach.

Phase 1: Python Prototyping (Current Focus)
Objective: Rapidly develop and test the core logic of agents and their interactions.

Language: Python

Core Libraries:

mesa: The primary framework for building the agent-based model structure, scheduler, and data collection.

pandas & geopandas: For loading, managing, and analyzing census and geographic data to initialize the simulation.

matplotlib & seaborn: For visualizing the simulation's output data (e.g., wealth distribution heatmaps).

Phase 2: Performance Optimization
Objective: Scale the simulation to handle tens of thousands of agents efficiently.

Problem: Python's performance will become a bottleneck in the main simulation loop, where every agent's state is updated in every step.

Solution:

The core simulation loop will be rewritten in a high-performance language (C++ or Rust).

Python will remain the high-level "controller." It will be used to set up the simulation, manage data, and call the high-performance engine.

Integration: A binding library like pybind11 (for C++) will be used to create a seamless interface between the Python controller and the C++/Rust engine.

6. Project Roadmap
M1: Basic Prototype: Implement Citizen and Business agents with simple behaviors (e.g., random movement, wealth exchange) on a grid. (This is the goal of the initial Python script).

M2: Economic Loop: Develop the core economic cycle: hiring, working, paying salaries, and consumption.

M3: Data Integration: Use real-world Birmingham data (census, geographic) to create a realistic initial state for the city and its agent population.

M4: Advanced Behaviors: Implement more sophisticated decision-making logic for agents (e.g., citizens choosing jobs based on salary and commute distance).

M5: Performance Migration: Profile the Python simulation, identify the main loop as the bottleneck, and rewrite it in C++/Rust, integrating it back into the Python framework.