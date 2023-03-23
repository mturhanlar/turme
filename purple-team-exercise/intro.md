# Intro

## Introduction <a href="#introduction" id="introduction"></a>

Purple teaming is a security methodology in which red and blue teams work closely together to maximize cyber capabilities through continuous feedback and knowledge transfer.

Purple teaming can help security teams to improve the effectiveness of vulnerability detection, threat hunting, and network monitoring by accurately simulating common threat scenarios and facilitating the creation of new techniques designed to prevent and detect new types of threats.

In every Purple Team there should be at least one:

* Red Team member which will run Tactics, Techniques, and Procedures (TTPs),
* Blue Team member who will identify and eradicate attacks of Red Team.

### Red Team <a href="#red-team" id="red-team"></a>

Responsible for emulating TTPs to test people, processes, and information assets.

The goal of the team is to make the Blue team better by training and measure whether blue team’s detection and response policies, procedures, and technologies are effective.

### Blue Team <a href="#blue-team" id="blue-team"></a>

They are the defenders, entrusted with identifying and remediating attacks and are generally associated with Security Operations Center or Managed Security Service Provider (MSSP), Hunt Team, Incident Response, and Digital Forensics.

The goal of the team is to identify, contain and eradicate attacks.



&#x20;

## Goals & Objectives <a href="#goals-and-objectives" id="goals-and-objectives"></a>

Our goal for doing the Purple Team exercise is:

* Test attack chains against Organizations.
* Test TTPs that have not been tested yet in Company.
* Filling the gaps between teams in terms of security.
* Preparation for a zero-knowledge Red Team Engagement.

&#x20;

&#x20;

## Purple Team Exercise Framework <a href="#purple-team-exercise-framework" id="purple-team-exercise-framework"></a>

&#x20;![](<../.gitbook/assets/image (5).png>)

## Methodology Description <a href="#methodology-description" id="methodology-description"></a>

The following framework (PTEF), created by industry experts (ref 1) , will be used during our purple team exercises at Organization, more explanation on each step can be found below.



### Cyber Threat Intelligence  <a href="#cyber-threat-intelligence" id="cyber-threat-intelligence"></a>

**(Decide Where and Decide Which TTPs )**

Cyber Threat Intelligence(CTI) is a team/group of people that research and provide adversary tactics, techniques, and procedures (TTPs) about a threat to an organization.

CTI is the first reason that Purple Team Programs are of high value. While a Red Team can try thousands of methods to reach an objective, a Purple Team will focus on the methods, tradecraft, and TTPs **that are most likely to impact the organization**.

In this phase, attendees will try to focus on which [Mitre Att\&ck](https://attack.mitre.org/) TTPs can be used during the exercise. The phase should give answers on which infrastructure the exercises will be and which TTPs will be run during the exercise.

Plan a meeting on this phase for 1 or 1,5 hours by trying to answer questions:

* Where will the exercise be?
* Which TTPs we can implement on these platforms?

This phase will help to form a detailed preliminary picture of the entity and its weak points from the attacker’s perspective. This will enable the Cyber Threat Intelligence to be put into context and will contribute to the development of the attack chains or attack scenarios to be used in the Purple Team Exercise.

&#x20;

### Preparation  <a href="#preparation" id="preparation"></a>

**(How?)**

The amount of meetings in this phase is directly correlated with TTPs' volume. The number of meetings is flexible.

Some of the must-have meetings are

* **Pitch**: First planning meeting, can be seen as a pitch of the exercise and purple team program. The meeting should introduce the concept of Purple Teaming, present the goals and objectives, and share this methodology
* **Planning meeting 1-x:** Once the exercise is accepted, several meetings can have place with respective stakeholders to ensure all actions are completed before the final meeting.\
  This may include but is not limited to:
  * Exercise Coordinator(s)
    * Ensuring physical location has met all the requirements including but not limited to power, connectivity to all systems (attack and defend infrastructure), A/C, etc.
    * Determine if tabletop discussions will occur before the exercise or the day of the exercise.
  * After the CTI meeting, Red Team Preparation and decide/presents Tools for the exercise.
  * Setup at least 2 systems to show attack activity.
  * Ensure Attack Infrastructure is fully functional.
  * Ensure Target Systems are accessible.
  * Document all commands required to emulate TTPs in a playbook.
  * Test TTPs before exercise on different hosts than the exercise hosts but that is configured exactly alike similarly.
  * Blue Team Preparation and decide/present Tools for the exercise
  * Validate security tools are reporting to production security tools from the target systems.
  * Ensure attack infrastructure is accessible through proxy/outbound controls.
  * Work with Red Team as payloads and C2 are tested before exercise on non-exercise systems.
  * Ensure the correct forensic tools are deployed on the target systems, if applies.
  * Install Live Forensic Tools for efficiency during Purple Team Exercise, if applies.
* **Final meeting**: Exercise Coordinator would lead the meeting and go over each action item assigned in previous meetings validating they are completed.

#### Metrics <a href="#metrics" id="metrics"></a>

Purple Team Exercises focus on Tactics, Techniques, and Procedures (TTPs) that do not follow the traditional vulnerability management metrics. A TTP, unlike a vulnerability, is not open or closed. It cannot be easily remediated by applying a patch. These are some of the metrics that may be discussed and agreed upon prior to the exercise:

* Data Sources
  * Logging events locally
  * Logging events centrally
* Detection
  * Alerts
  * Telemetry
  * Indicator of Compromise
  * General Behavior
  * Specific Behavior
* Response
  * Time to Detect
  * Time to Investigate
  * Time to Remediate

[DETT\&CT](https://github.com/rabobank-cdc/DeTTECT) tool can be used.

&#x20;

### Execution  <a href="#execution" id="execution"></a>

**(Kick Off)**

&#x20;

1. Exercise Coordinator or Cyber Threat Intelligence analysts presents the TTPs, and technical details. This can be a series of slides that introduce the adversary, previous targets, known behaviors, tools, attack vectors leveraged, and the tactics, techniques, and procedures.
2. Red Team executes the TTP while sharing the screen and providing all indicators of compromise and behaviors:
   * Provide attacker IP
   * Provide target
   * Provide exact time
   * Shows the attack to attendees
3. Blue Team will follow standard process to identify evidence of TTP. Time should be monitored to meet expectations and move exercise along. Exercise Coordinator should share screen if TTP was identified due to alerts, threat hunting for logs, or any other evidence. Metrics should be taken and documented.
4. Document any output, action items, or notes for the TTP.
5. Repeat for next TTP.

Most purple team exercises can be between 1 - 5 days.

&#x20;

To keep track on TTPs during the exercise, the following table is useful to be used.

| **Tactic**     | **Technique**                                                                  | **Procedure**                                        | **CTI Reference** |
| -------------- | ------------------------------------------------------------------------------ | ---------------------------------------------------- | ----------------- |
| Description    | Technique/SubTechnique or Mittre Att\&ck ID                                    |                                                      |                   |
| Reconnaissance | <p>Active Scanning/Scanning IP Blocks</p><p><strong>ID:</strong> T1595.002</p> | It could be attached to an APT or it can be generic. |                   |
| Initial Access | <p>Phishing/Spearphishing Attachment</p><p><strong>ID:</strong> T1566.001</p>  | It could be attached to an APT or it can be generic. |                   |
| ….             | ….                                                                             |                                                      |                   |

[VECTR](https://docs.vectr.io/) tool can be used as well.

&#x20;

### Lessons Learned <a href="#lessons-learned" id="lessons-learned"></a>

Exercise Coordinators are responsible for taking minutes, notes, action items, and attendee/sponsor feedback. The Exercise Coordinators are also responsible for the creation of a Lessons Learned document (Report) following each exercise.

For the Purple Team Exercise itself, feedback requests should be sent to all attendees on the last day of the Purple Team Exercise to obtain immediate feedback while it is fresh on attendees’ minds.

In that phase, we should schedule a meeting, talk and decide on the following questions.

* Which of TTPs did we execute?
* Could we detect them?
* If not, how we can detect and how we can improve the current situation.
* When and where will be the next exercise.

&#x20;

## Roles and Responsibilities <a href="#roles-and-responsibilities" id="roles-and-responsibilities"></a>



| **Title**                              | **Role**         | **Responsibility**                                                                   |
| -------------------------------------- | ---------------- | ------------------------------------------------------------------------------------ |
| Head of Security                       | Sponsor/Attendee | Approve Purple Team Exercise                                                         |
| SOC Team Manager(Exercise Coordinator) | Sponsor/Attendee | Preparation: Defining goals for both Red and Blue teams, Select Attendees            |
| Red Team                               | Attendee         | Coordinating TTPs during the Cyber Threat Intelligence phase, Preparation, Execution |
| Blue Team - SOC                        | Attendee         | Coordinating TTPs during the Cyber Threat Intelligence phase, Preparation, Execution |

## Tools in Purple Team Exercise <a href="#tools-in-purple-team-exercise" id="tools-in-purple-team-exercise"></a>

### Red Team Tools <a href="#red-team-tools" id="red-team-tools"></a>

The red Team should decide which tools are more appropriate for the exercise.

The C2 matrix can be used for that [![](https://uploads-ssl.webflow.com/5da7611163ad233795868d9d/5dd3471ce192cdcf77b7320d\_c2favicon.png)The C2 Matrix](https://www.thec2matrix.com/).

### Blue Team Tools <a href="#blue-team-tools" id="blue-team-tools"></a>

[https://github.com/rabobank-cdc/DeTTECT](https://github.com/rabobank-cdc/DeTTECT)

### Documentation <a href="#documentation" id="documentation"></a>

[https://docs.vectr.io/](https://docs.vectr.io/)



## Reference <a href="#references" id="references"></a>

1. [https://github.com/scythe-io/purple-team-exercise-framework/blob/master/PTEFv2.md](https://github.com/scythe-io/purple-team-exercise-framework/blob/master/PTEFv2.md)
2. [![](https://uploads-ssl.webflow.com/5da7611163ad233795868d9d/5dd3471ce192cdcf77b7320d\_c2favicon.png)The C2 Matrix](https://www.thec2matrix.com/)
3. [![](https://www.redscan.com/favicon-16x16.png)What is Purple Teaming? How Can it Strengthen Your Security? | Redscan](https://www.redscan.com/news/purple-teaming-can-strengthen-cyber-security/)
4. [![](https://miro.medium.com/v2/1\*m-R\_BkNf1Qjr1YbyOIJY2w.png)Ethical Hacking Definitions](https://medium.com/@jorgeorchilles/ethical-hacking-definitions-9b9a6dad4988)

\
