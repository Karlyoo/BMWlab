## 9/10
Ian suggested to study Summer's paper  this month,focusing in SMO and RIC. And if there's problem with it,I can ask for Tobby's or Ian's help.
For the Research Topic,it seems like there 's no one's topic is closed to the MIMO scheduling. So maybe I should focus on OAI first. And having a meeting with Prof later.

## 10/1
I ask Tobby for E2SM-KPM installation. And he mentioned that the gNB and RIC can not run in the same machine. So maybe I can install by myself one time,then cooperate with Henry and Richard(林孝陽) to finish this. 

Also Tobby and Richard gave us some advice for Eurecom Intership,including the neccessary document and the flight.

## 10/15
For the OAI gnb and [L] near RT RIC,I can use o-cloud OAI gnb by Nino to try to connect it. My next step is install it by Nino's installation guide.
And Johnson advices me to use OAI UE to confirm  and test the connection first.



## 11/10 meet with Ian:

## 2025/11/11

- **Recording**: [./records/a0870b5b-0a22-49fe-b292-e6e5a6651419.vtt](a0870b5b-0a22-49fe-b292-e6e5a6651419)
- **Attendees**:
  - Karl (Speaker)
  - Ian (Listener)
- **Topic**: Technical Project Review, Workflow Guidance, and Deadline Reinforcement
- **Summary**: Ian conducted a detailed review of the Karl's project status, focusing on duplicating "Summer's" previous work on automatic deployment (SMO). The session addressed a critical OAI-GNB connection issue, provided explicit instructions for Trello/repository updates, and mandated a workflow change from the GitHub UI to Visual Studio Code for editing documentation. Key project deadlines for December (internship) and June (final presentation) were emphasized.

- **Discussion Points**:
  - **Project Task & Status (SMO/OAI)**
    - The primary objective is to duplicate the "automatic deployment" and "SMO" (Service Management and Orchestration) task previously completed by "Summer."
    - The immediate, first-priority target is to "verify whether you can collect data or not." The speaker noted that the platform (VM, Docker, Kubernetes) is less important than achieving this first step.
    - The attendee confirmed a major blocker: they "cannot connect with OAI."
    - The connection issue was specifically identified as an inability to connect to the 5G core network, with the speaker pointing to the OAI-GNB as the problem.

  - **Trello & Repository Workflow**
    - All work must be pushed to the "lab's repository."
    - The Trello board (referred to as "trail log") must be kept in sync.
    - Specifically, the attendee must "attach the hyperlink" from the repository to the "digital twin" Trello card to "fulfill these action items."

  - **Documentation & Development Environment**
    - Ian strongly advised against editing "study notes" (Markdown documentation) directly in the GitHub web interface, stating it "will waste your time so much."
    - Karl was instructed to use **Visual Studio Code** to edit documentation locally, as it is "really quick" and more efficient.
    - The speaker provided detailed, specific feedback on Markdown formatting, including:
      - Using double spaces for proper line breaks.
      - Correctly defining shell code blocks (e.g., `shell`) to ensure commands are "safe" and properly commented.
      - Using specific formatting (e.g., "green one" for errors) for clarity.

  - **Account & Access Issues**
    - Karl appears to be working in their "private repository," not the required "lab" repository.
    - Ian identified a probable account issue, instructing the Karl to "check your GitHub education" access.
    - This account problem is likely the reason the attendee "cannot access the organization repository" and is missing "premium accounts" features, which is a critical blocker.

  - **Project Deadlines & Context**
    - The current task (SMO, OAI-GNB integration) **must be finished before December**.
    - This deadline is critical because it's tied to an internship at  EURECOM next semester.
    - The final project results must be presented in **June next year**.

- **Action Items**:
  - Karl:
    - [ ] Prioritize duplicating Summer's "automatic deployment" and "SMO" work.
    - [ ] Focusing on immediate goal of verifying data collection.
    - [ ] Troubleshoot and fix the OAI connection issue, specifically with the 5G core and the OAI-GNB.
    - [ ] Move all project work from the private repository to the correct "lab's repository."
    - [ ] Stop using the GitHub web UI for editing documentation and use Visual Studio Code to edit the "study notes" locally.
    - [ ] Resolve the GitHub account issue (check "GitHub education" status) to gain access to the "organization repository."

  
