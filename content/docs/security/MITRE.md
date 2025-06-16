## ATT&CK Framework
According to the website, "MITRE ATT&CK® is a globally-accessible knowledge base of adversary tactics and techniques based on real-world observations." 

In 2013, MITRE began to address the need to record and document common TTPs (Tactics, Techniques, and Procedures) that APT (Advanced Persistent Threat) groups used against enterprise Windows networks. 

This started with an internal project known as FMX (Fort Meade Experiment). Within this project, selected security professionals were tasked to emulated adversarial TTPs against a network, and data was collected from the attacks on this network. The gathered data helped construct the beginning pieces of what we know today as the ATT&CK® framework.

The ATT&CK® framework has grown and expanded throughout the years. One notable expansion was that the framework focused solely on the Windows platform but has expanded to cover other platforms, such as macOS and Linux. The framework is heavily contributed to by many sources, such as security researchers and threat intelligence reports. Note this is not only a tool for blue teamers. The tool is also useful for red teamers.

https://attack.mitre.org/

Direct your attention to the bottom of the page to view the ATT&CK® Matrix for Enterprise. Across the top of the matrix, there are 14 categories. Each category contains the techniques an adversary could use to perform the tactic. The categories cover the seven-stage Cyber Attack Lifecycle (credit Lockheed Martin for the Cyber Kill Chain).

## MITRE ENGAGE

https://engage.mitre.org/

(Source: https://engage.mitre.org)
https://engage.mitre.org/wp-content/uploads/2022/04/EngageHandbook-v1.0.pdf
Let's quickly explain each of these categories based on the information on the Engage website.

- Prepare the set of operational actions that will lead to your desired outcome (input)
- Expose adversaries when they trigger your deployed deception activities 
- Affect adversaries by performing actions that will have a negative impact on their operations
- Elicit information by observing the adversary and learn more about their modus operandi (TTPs)
- Understand the outcomes of the operational actions (output) 
 
## MITRE ENGENUITY

 CTID

MITRE formed an organization named The Center of Threat-Informed Defense (CTID). This organization consists of various companies and vendors from around the globe. Their objective is to conduct research on cyber threats and their TTPs and share this research to improve cyber defense for all. 

Some of the companies and vendors who are participants of CTID:

    AttackIQ (founder)
    Verizon
    Microsoft (founder)
    Red Canary (founder)
    Splunk

Per the website, "Together with Participant organizations, we cultivate solutions for a safer world and advance threat-informed defense with open-source software, methodologies, and frameworks. By expanding upon the MITRE ATT&CK knowledge base, our work expands the global understanding of cyber adversaries and their tradecraft with the public release of data sets critical to better understanding adversarial behavior and their movements."

Adversary Emulation Library & ATT&CK® Emulations Plans

The Adversary Emulation Library is a public library making adversary emulation plans a free resource for blue/red teamers. The library and the emulations are a contribution from CTID. There are several ATT&CK® Emulation Plans currently available: APT3, APT29, and FIN6. The emulation plans are a step-by-step guide on how to mimic the specific threat group. If any of the C-Suite were to ask, "how would we fare if APT29 hits us?" This can easily be answered by referring to the results of the execution of the emulation plan. 

---

Per the D3FEND website, this resource is "A knowledge graph of cybersecurity countermeasures."
	D3FEND is still in beta and is funded by the Cybersecurity Directorate of the NSA. 
	D3FEND stands for Detection, Denial, and Disruption Framework Empowering Network Defense. 

---
Cyber Analytics Repository
https://car.mitre.org/

The MITRE Cyber Analytics Repository (CAR) is a knowledge base of analytics developed by [MITRE](https://www.mitre.org) based on the [MITRE ATT&CK](https://attack.mitre.org/) adversary model. CAR defines a data model that is leveraged in its pseudocode representations, but also includes implementations directly targeted at specific tools (e.g., Splunk, EQL) in its analytics. With respect to coverage, CAR is focused on providing a set of validated and well-explained analytics, in particular with regards to their operating theory and rationale.