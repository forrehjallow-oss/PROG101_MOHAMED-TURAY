# PROG101_MOHAMED-TURAY
 It proposes a triage and queue system for Community Health Centres that sorts patients into Emergency, Urgent and Routine queues using Boolean rules, always calls the most critical patient first, and uses a fairness limit so Routine patients aren't left waiting forever. The report includes an algorithm

#Overview
MediQueue SL is a design-only system (algorithm, pseudocode, flowcharts, no code) for Community Health Centres in Sierra Leone. A nurse enters a patient's age, sex, temperature, days ill and a few Yes/No danger signs. The system assigns a priority, issues a ticket with an estimated wait, calls the next patient, flags suspected infectious cases, and produces a daily summary. It works offline on low-cost hardware.



##Problem
Patients are seen in the order they sign a paper register, not by medical need. Critically ill patients (a convulsing child, a bleeding pregnant woman) wait alongside people with mild complaints. Causes include too few health workers, handwritten records, unreliable electricity and internet, and no simple triage tool.

##Solution
Boolean rules sort patients into Emergency (red), Urgent (yellow) or Routine (green) queues.
Emergency patients are always called first.
A fairness limit (4 Urgent calls, then 1 Routine) prevents Routine patients waiting forever.
Suspected infectious cases are flagged for isolation.
Patients get an estimated wait time, and managers get daily reports.



##SDG 3 (Good Health and Well-being): targets 3.1 (maternal mortality), 3.2 (child deaths) and 3.8 (universal health coverage).
SDG 10 (Reduced Inequalities): care is based on need, not wealth or influence.
SDG 9 (Industry, Innovation and Infrastructure): encourages low-cost, locally built digital innovation.





Constants, Records and Global Variables
CONSTANT Integer EMERGENCY = 1, URGENT = 2, ROUTINE = 3 
CONSTANT Integer MAX_QUEUE_SIZE = 200, FAIRNESS_LIMIT = 4, AVG_CONSULT_MINUTES = 
10 
CONSTANT Integer CHILD_AGE_LIMIT = 5, ELDERLY_AGE_LIMIT = 65, LONG_ILLNESS_DAYS = 3 
CONSTANT Real FEVER_LIMIT = 38.0, HIGH_FEVER_LIMIT = 39.5 
RECORD Patient 
Integer queueNo, age, days, priority 
String name, sex 
Real temperature 
Boolean unconscious, severeBleeding, breathingDifficulty, pregnant, dangerSign, 
suspectedInfectious, isolation 
END RECORD 
Patient emergencyQueue[MAX_QUEUE_SIZE], urgentQueue[MAX_QUEUE_SIZE], 
routineQueue[MAX_QUEUE_SIZE] 
Integer eCount = 0, uCount = 0, rCount = 0, totalRegistered = 0, servedCount = 0 
Integer consecutiveUrgent = 0, eTotal = 0, uTotal = 0, rTotal = 0, infectiousCount = 0 Boolean 


 choice 
WHILE running = TRUE DO DisplayMenu() 
choice = GetValidatedInteger("Enter choice", 1, 5) 
SELECT CASE choice 
CASE 1: RegisterPatient() 
CASE 2: CallNextPatient() 
CASE 3: ShowQueueStatus() 
CASE 4: GenerateDailySummary() 
CASE 5: running = FALSE 
END SELECT 
END WHILE 
DISPLAY "System closed. Stay safe." END 
MODULE 
MODULE DisplayMenu() 
DISPLAY "MediQueue SL: 1 Register, 2 Call next, 3 Queue status, 4 Daily summary, 5 Exit" 
END MODULE 
FUNCTION Integer GetValidatedInteger(String prompt, Integer low, Integer high) Integer 
value 
REPEAT 
DISPLAY prompt; INPUT value 
IF value < low OR value > high THEN DISPLAY "Invalid. Enter " + low + " to " + high 
UNTIL value >= low AND value <= high RETURN 
value 
END FUNCTION
GetValidatedReal(prompt, low, high) has the same structure for Real values. 
FUNCTION Boolean GetYesNo(String prompt) 
String answer 
REPEAT 
DISPLAY prompt + " (Y/N)"; INPUT answer; answer = UPPERCASE(answer) 
UNTIL answer = "Y" OR answer = "N" 
RETURN (answer = "Y") 
END FUNCTION 
3.3 Registration, Priority and Queue Modules
MODULE RegisterPatient() Patient 
p; Integer waitMinutes 
p.queueNo = totalRegistered + 1 
DISPLAY "Patient name and sex (F or M):"; INPUT p.name, p.sex p.age 
= GetValidatedInteger("Age in years", 0, 120) 
p.temperature = GetValidatedReal("Temperature in C", 30.0, 45.0) 
p.days = GetValidatedInteger("Days ill", 0, 365) 
p.unconscious = GetYesNo("Unconscious or convulsing?") 
p.severeBleeding = GetYesNo("Severe bleeding?") 
p.breathingDifficulty = GetYesNo("Difficulty breathing?") 
p.suspectedInfectious = GetYesNo("Signs of cholera, Lassa fever or Ebola?") 

