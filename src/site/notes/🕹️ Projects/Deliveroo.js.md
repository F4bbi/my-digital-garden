---
{"dg-publish":true,"permalink":"/projects/deliveroo-js/","created":"2025-03-29T18:20:22.522+01:00","updated":"2025-10-01T02:44:17.521+02:00"}
---

# Deliveroo.js

[Deliveroo.js](https://github.com/F4bbi/autonomus-software-agents) is a project developed for the **Autonomous Software Agents** course at the University of Trento during the 2024/2025 academic year, by me and Alessandro Sartore.

The project focuses on implementing an autonomous agent using the **Belief-Desire-Intention (BDI)** model, which enables the agent to perceive its environment, set goals, plan actions, and continuously adapt to dynamic conditions. More about the BDI model [here](https://en.wikipedia.org/wiki/Belief%E2%80%93desire%E2%80%93intention_software_model).

The agents operate in a simulated environment where they must **pick up and deliver parcels efficiently**, as illustrated below.

In addition, the project consists of two parts.

## Single Agent

A standalone agent that independently explores the environment, updates its beliefs, and makes decisions to achieve its goals.

[![Single Agent Example|1000](https://github.com/F4bbi/autonomus-software-agents/raw/main/assets/single_agent.gif)](https://github.com/F4bbi/autonomus-software-agents/blob/main/assets/single_agent.gif)

## Multi-Agent

A system of **two cooperating agents** that share information, coordinate strategies, and divide tasks to complete deliveries more effectively.  

[![Multi-Agent Example|1000](https://github.com/F4bbi/autonomus-software-agents/raw/main/assets/multi_agent.gif)](https://github.com/F4bbi/autonomus-software-agents/blob/main/assets/multi_agent.gif)

Performance was evaluated across multiple game levels. Detailed results and insights can be found in the [project report](https://github.com/F4bbi/autonomus-software-agents/blob/main/docs/report.pdf) (_Evaluation and Results_ section) and in the [presentation slides](https://github.com/F4bbi/autonomus-software-agents/blob/main/docs/presentation.pdf) (very proud of them :) ).

Thanks to the strategies we developed, our team achieved **2nd place** in the final competition against other agents!

The simulation environment is based on the [Deliveroo.js framework](https://github.com/unitn-ASA/DeliverooAgent.js), developed by the course professor.