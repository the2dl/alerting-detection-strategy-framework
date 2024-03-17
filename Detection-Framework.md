# Detection Name and Reference
Recommended detection name to be created. This can be any format you decide works best for your organization. 

For reference, the GUID associated to this testing is **unique-guid** in SSDT.

# Goal
The goal is the intended purpose of the alert. It is a simple, plaintext description of the type of behavior you're attempting to detect.

# MITRE Categorization
The categorization is a correlation between the detection and the applicable entry in the MITRE Adversarial Tactics, Techniques, and Common Knowledge (ATT&CK) Framework. ATT&CK supplies a standard for various post-exploitation strategies and methods that adversaries can employ.

Mapping to the ATT&CK framework makes it possible to conduct further investigation into the technique, delivers a reference to the areas of the kill chain where the detection will be utilized, and can additionally facilitate insight and metrics into alert gaps. In our environment, we possess a knowledge base that correlates all of our detections to individual components of the MITRE ATT&CK framework. When forming a hypothesis for a new alert, an engineer can simply evaluate where we are strongest — or weakest — in relation to individual ATT&CK techniques.

When selecting a MITRE ATT&CK category, please select both the main and subordinate categories (e.g. Credential Access / Brute Force). Additionally, please ensure to highlight the MITRE DS that is utilized for the log source.

# Detection Functionality
The detection functionality outlines a high-level walkthrough of how the detection functions. This describes what the alert is looking for, what technical data sources are used, any enrichment that occurs, and any false positive minimization steps.

# Blind Spots and Assumptions
Blind Spots and Assumptions are the recognized issues, assumptions, and areas where an detection may not fire. No detection is perfect and identifying assumptions and blind spots can help other engineers understand how an detection may fail to fire or be defeated by an adversary.

# False Positives
False Positives are the known instances of an detection misfiring due to a misconfiguration, idiosyncrasy in the environment, or other non-malicious scenario. The False Positives section notes uniqueness to your own environment, and should include the defining characteristics of any activity that could generate a false positive alert. These false positive alerts should be suppressed within the SIEM to prevent alert generation when a known false positive event occurs.

Each alert / detection strategy needs to be tested and refined to remove as many false positives as possible before it is put into production.

False positive minimization relies on looking at several principles of the strategy and making adjustments, such as:

* Add an additional component to the rule to maximize true positives.
* Remove common false positives through patterns.
* Back-end filtering to store indices of expected false positives.

Ideally, you want your strategy to have the fewest false positives possible while maintaining the spirit of your rule. If a low false positive rate cannot be reached, the alert may need to be broken down, refactored, or entirely discarded.

The one exception to this is for Risk Analytics, if the goal of the detection is to be somewhat noisy (maybe 500 detections per week), make sure to highlight that when outlining the rule, and adjust priority accordingly. 

# Validation
Validation are the steps required to generate a representative true positive event which triggers this alert. This is similar to a unit test and describes how an engineer can cause the detection to fire. This can be a walkthrough of steps used to generate an alert, a script to trigger the detection (such as Red Canary's Atomic Red Team Tests), or a scenario used in an alert testing and orchestration platform.

Each alert / detection strategy must have true positive validation. This is a testing process designed to prove the true positives are detected.

True positive validation relies on generating a scenario in which the detection strategy is testing, and then validating in the tool.

To perform positive validation:

* Generate a scenario where a true positive would be generated.
* Document the process of your testing scenario.
* From a testing device, generate a true positive alert.
* Validate the true positive alert was detected by the strategy.

If you are unable to generate a true positive alert, the alert may need to be broken down, refactored, or entirely discarded.

# Relevance
The determination of a detection's relevance is pivotal in substantiating its creation and subsequent onboarding. Various factors, such as novel techniques, outputs derived from internal or external red team operations, commonly encountered attacks targeting LOLBAS, and CTI reporting, can serve as justifiable grounds for the development of a new detection.

# Priority
The concept of priority denotes the different levels of urgency that can be assigned to a detection. While the alert itself should accurately reflect the priority level when triggered through configuration within your security information and event management (SIEM) system (e.g., High, Medium, Low), this section outlines the specific criteria used to determine each priority level.

It is essential to ensure that the priority levels align with the risk scoring ranges defined for your organization to effectively prepare for risk analytics.

# Search Example   
To initiate the detection process, a comprehensive initial search query is essential. This query should encompass as many relevant details as possible, ensuring a broader scope for detection. Additionally, identifying potential tuning opportunities within the query can enhance its effectiveness and accuracy.