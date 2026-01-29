# CUJ Framework Document

## User Goal and Persona

**Persona**  
A busy instructor managing an in-person class of approximately 40 students giving group presentations.

**Value-Driven Goal**  
To conduct in-class presentations **fairly, efficiently, and with minimal cognitive overhead**, allowing the instructor to focus on evaluating presentations rather than managing logistics.

---

## The Journey Audit

### Step-by-Step Actions (Happy Path)

1. Download or clone the project code from GitHub  
2. Set up the application using terminal commands  
3. Open the browser and navigate to the app URL  
4. Verify that the list of teams is correct  
5. Click the **Randomize** button to select the next team  
6. Announce the selected team to the class  
7. Start the presentation timer  
8. Monitor the 2-minute warning  
9. Start the Q&A timer  
10. End Q&A and proceed to the next team by clicking **Randomize**  
11. Repeat until all teams have presented  

---

### Unhappy Path Scenarios

- The instructor forgets to start the Q&A timer, or finds the transition between timers not intuitive (there is no hint before nor between the switch) (refer to picture "timer switching to Q&A phase.png")
- A selected team is temporarily unavailable (e.g., technical issues or bathroom break), there is no way to skip a group (refer to "cannot skip a selected team.png")
- The instructor loses track of whether the timer has been started or not
- A new user is unsure whether the timer section includes a Q&A phase (refer to "landing page : initial stage.png")

---

### Context Switches

**Physical Context Switches (Leaving the App)**

- Switching to a presentation rubric to evaluate the current team  
- Checking slides or lecture notes in a separate window
- Looking at a phone or watch to confirm elapsed time if unsure whether the timer is running

**Mental Context Switches (Within the App)**

- Interpreting whether the current timer is for **Presentation** or **Q&A**  
- Remembering whether the timer needs to be manually restarted between phases
- Inferring the meaning of visual cues (e.g., greyed-out team tags indicating “presented”)
- Re-orienting after interruptions such as student questions or technical delays

These context switches increase cognitive load and raise the likelihood of timing errors, especially when repeated across many teams in a single session.

---

### Time Tracking

**One-Time Setup**

- Download / clone the code from GitHub: ~1 minute  
- Set up using terminal commands: ~30 seconds  
- Open browser and navigate to the app: ~10 seconds  
- Verify team list: ~30 seconds  

**Per-Team Cycle**

- Click the **Randomize** button: ~1 second  
- Announce selected team: ~10 seconds 
- Start presentation timer: ~1 second  
- Monitor presentation and warning period: ~2 minutes  
- Start Q&A timer: ~1 second

Small delays accumulate over many iterations, making friction more noticeable during longer sessions.

---

### Evidence

- Landing page (initial state)
- Presentation timer with 2-minute warning active  
- Transition from presentation phase to Q&A phase  
- Q&A timer completed (session end state)  
- All teams marked as presented  

---

## Analysis & Recommendations

### Highlights / Lowlights

**Great**

- Visual marking of “presented” teams significantly reduces mental bookkeeping  
- Randomized selection improves perceived fairness  
- The 2-minute warning is clear and easy to notice  

**Moderate**

- Timer controls require conscious attention and manual interaction  
- Phase switching (Presentation vs Q&A) is not immediately obvious to first-time users

**Severe**

- If the instructor forgets to start a timer, the app provides no safeguard, which can lead to uneven timing and perceived unfairness  
- New users may not realize that a Q&A timer exists after the presentation timer  
- The meaning of greyed-out team tags is not self-evident without prior explanation  

---

### Product Recommendations

1. **Phase-Aware Prompt**  
   After the presentation timer ends, display a clear visual cue such as  
   *“Next event: Q&A”* to reduce confusion and missed transitions.

2. **Session-State Indicator**  
   Add a lightweight progress indicator (e.g., *“3 / 10 teams presented”*) to help users quickly re-orient after interruptions.

---

## Future User Advice (Pro Tips)

- Pre-load and verify the team list before class begins  
- Use the app in full-screen mode to minimize distractions  
- Keep the app open between presentations to avoid re-orienting or reinitializing the session  