# Agent Building 
## Overview

#### This room teaches how to build a Security Investigation Agent step by step using Python and the TryHackMe AI service. The agent investigates SIEM alerts by retrieving evidence, correlating logs, checking IP reputation, consulting organisation context, and maintaining conversation memory — while keeping final decisions with the human analyst.
### Task 1: Manual Investigation (SIEM Basics)

Scenario: Investigate alert ALT-001 (Successful Office Sign-in) for Maria Stow manually before building the agent.

Pivots used:

    Account: maria.stow@northstar.fashion

    Source IP: 198.51.100.24

    Device: NS-LT-002

    Event: login_success

    Time: 07:52:04 UTC

### SIEM Access:

    URL: http://10.80.140.160:8000/

    Operator: analyst / Passphrase: analyst123

Query refinement process:
Query	Logs Returned
Account only	49
Account + Source IP	44
Account + Source IP + Device	21
Questions & Answers

    How many logs were returned when searching only Maria's account? → 49

    How many logs remained after adding the source IP? → 44

    How many logs remained after adding the device? → 21


### Task 2: Create the First Agent (No Tools)

Goal: Build the initial agent with instructions and boundaries — but no SIEM access yet.

Key file: agent_task3.py

Agent instructions define:

    Role: SOC assistant for NorthStar Fashion

    Allowed verdicts: TruePositive, BenignPositive, FalsePositive, InsufficientEvidence

    Forbidden actions: closing alerts, blocking IPs, disabling accounts, containment, running commands



    client = THMAgentClient()

    AGENT_INSTRUCTIONS = (
    "You are the Security Investigation Agent for NorthStar Fashion, a SOC "
    "assistant. Analyse only the alert and evidence supplied in this "
    "conversation - never invent SIEM data. "
    "Use only these verdicts: TruePositive, BenignPositive, FalsePositive, "
    "or InsufficientEvidence. Respond with a Verdict, Key Evidence, a "
    "one-sentence Reason, and a Recommendation. "
    "You cannot close alerts, change SIEM state, perform containment, block "
    "IP addresses, disable accounts, or run commands - you only support the "
    "human investigation."
    )

    response = client.send_message(
    f"{AGENT_INSTRUCTIONS}\n\nUser request: {investigation_request}"
    )


Result: The agent recognises ALT-051 as an alert ID but cannot retrieve it — it correctly returns InsufficientEvidence because it has no tools.

Key principle: Reasoning about a capability does not grant access to that capability.
Questions & Answers

    Which method sends a message to the AI service? → send_message

    Which response field contains the generated assistant text? → content

    Can the agent retrieve ALT-051 from the SIEM at this stage? → Nay

### Task 3: Add Alert Capabilities (list_alerts, get_alert)

Goal: Build the execution layer that maps AI tool requests to approved Python functions.

Key file: agent_task4.py

API endpoints used:

    GET /services/siem/alerts — list alerts (params: count, offset)

    GET /services/siem/alerts/{id} — retrieve one alert

    Header: X-SIEM-API-Key: sk_live_demo_9f3a21c4b77d4d3e

Functions built:
python

def get_alert(alert_id: str) -> dict:
    response = requests.get(
        f"http://127.0.0.1:8000/services/siem/alerts/{alert_id}",
        headers={"X-SIEM-API-Key": siem_api_key},
        timeout=5,
    )
    response.raise_for_status()
    return response.json()

APPROVED_CAPABILITIES = {
    "list_alerts": list_alerts,
    "get_alert": get_alert,
}

Agent loop:

    Send message to AI service

    Parse tool request (parse_tool_call)

    Check APPROVED_CAPABILITIES allowlist

    Execute approved function (run_tool_call)

    Return result as TOOL_RESULT: <json>

    Repeat (max 5 tool calls per turn)

Key principle: The model proposes. The application enforces.
Questions & Answers

    Which query parameter controls how many alerts are skipped? → offset

    Which tool retrieves one complete alert by its ID? → get_alert

### Task 4: Add Log Search (search_logs)

Goal: Allow the agent to pivot from alerts into related SIEM logs.

Key file: agent_task5.py

API endpoint:

    POST /services/siem/search (JSON body: search, count, offset)

Normalised SIEM fields:
Field	Identifies
actor.principal_email	Account performing activity
target.principal_email	Account affected
network.source_ip	Source IP
device.device_name	Device
normalized_event	Normalised event type
native_event_name	Original event name
outcome	Success/failure

Code added:
python

response = requests.post(
    "http://127.0.0.1:8000/services/siem/search",
    headers={"X-SIEM-API-Key": siem_api_key},
    json={"search": query, "count": count, "offset": offset},
    timeout=5,
)

APPROVED_CAPABILITIES = {
    "list_alerts": list_alerts,
    "get_alert": get_alert,
    "search_logs": search_logs,
}

Handling large results:

    FIELDS_TO_SKIP removes metadata (raw_log, schema_version, etc.)

    remove_empty_fields() cleans nested structures

    build_tool_result_message() keeps messages under 4096 chars, progressively truncating results if needed

Key principle: Correlation, not collection — retrieve evidence that answers a specific question.
Questions & Answers

    Which logical operator requires both search conditions to match the same log? → AND

    Which HTTP method does the SIEM search endpoint use? → POST

### Task 5: Add External & Organisation Context

Goal: Add IP reputation checking and internal knowledge base lookup.

Key file: agent_task6.py
IP Reputation Service

    GET /api/v2/check (header: Key: demo-key)

    Params: ipAddress, maxAgeInDays, verbose

    Returns: abuseConfidenceScore, totalReports, numDistinctUsers, lastReportedAt, reports

python

def check_ip_abuse(ip_address: str) -> dict:
    response = requests.get(
        "http://127.0.0.1:8000/api/v2/check",
        headers={"Key": abuseipdb_api_key, "Accept": "application/json"},
        params={"ipAddress": ip_address, "maxAgeInDays": 90, "verbose": ""},
        timeout=5,
    )

Organisation Context

    Local Markdown files in org_details/

    Deterministic keyword matching (no embeddings)

    MIN_MATCH_SCORE = 2

    Returns source + text

python

APPROVED_CAPABILITIES = {
    "list_alerts": list_alerts,
    "get_alert": get_alert,
    "search_logs": search_logs,
    "check_ip_abuse": check_ip_abuse,
    "search_org_details": search_org_details,
}

Evidence source → question mapping:
Evidence Source       	Question Answered
Alert                 	Why was the detection created?
SIEM logs	              What activity actually occurred?
IP reputation         	Has the source IP been reported elsewhere?
Organisation context	  Is there internal context that changes the interpretation?

Key principle: Evidence can change interpretation — a BenignPositive means the activity occurred but was legitimate/authorised.
Questions & Answers

    Which capability checks an IP address for previously reported abusive activity? → check_ip_abuse

    Which capability searches NorthStar Fashion's internal organisation documents? → search_org_details

### Task 6: Conversation Memory

Goal: Enable follow-up questions without repeating the investigation.

Key file: agent_task7.py

How memory works:

    The TryHackMe AI service preserves conversation history server-side

    client.send_message(...) continues the same conversation automatically

    client.get_messages() retrieves the stored conversation history

    No LangGraph checkpointer or thread ID required

History command code:
python

if human_msg.strip().lower() == "history":
    try:
        history = client.get_messages()
    except THMAgentError as error:
        print(f"AI request failed: {error}")
        continue

    for entry in history.get("messages", []):
        print(f"[{entry['role']}] {entry['content']}")
    continue

Example history output:
text

[user] Investigate alert ALT-049.
[assistant] ...
[user] TOOL_RESULT: ...
[assistant] ...

Key concepts:

    Conversation memory preserves what happened during the current investigation

    Organisation context provides reusable internal knowledge

    Memory improves continuity, but useful memory needs clear boundaries (session, case, incident, investigation)

    client.clear_messages() clears the shared history — only use when explicitly required or to recover from a stuck conversation

Key principle: Using memory and inspecting memory are separate operations.
Questions & Answers

    Which method retrieves the conversation history stored by the TryHackMe AI service? → get_messages

    Does the agent need a separate LangGraph checkpointer to remember this conversation? → Nay

    Which file contains the completed Security Investigation Agent built throughout the room? → agent_complete.py

Complete Agent Summary

Final file: agent_complete.py

Five approved capabilities:

    list_alerts — browse alerts in pages

    get_alert — retrieve one complete alert

    search_logs — query normalised SIEM logs

    check_ip_abuse — external IP reputation

    search_org_details — internal organisation knowledge

Progression across tasks:
Task	Added Capability
Task 3	Define agent behaviour + connect to AI
Task 4	Execute approved alert capabilities
Task 5	Correlate alerts with SIEM logs
Task 6	Add external and organisation context
Task 7	Continue investigations across follow-up questions
Key Takeaways

    Build agent capabilities incrementally — test each function before exposing it as a tool so requests, returned data, and failures are easier to understand before the model decides when to use them.

    Treat every source as evidence with limits — alerts explain detections, logs show related activity, IP reputation adds external context, and organisation documents may establish authorisation, but no single source should be treated as proof without corroboration.

    Keep investigation support and security action separate — the Security Investigation Agent may gather evidence and recommend a verdict, but the engineer remains responsible for the final decision, alert handling, and containment actions.

    The model proposes. The application enforces. — The AI requests capabilities via structured JSON; the application checks APPROVED_CAPABILITIES and executes only what is allowed.

    Correlation, not collection — focused searches improve investigation relevance, context efficiency, response quality, and explainability.

    Memory improves continuity, but useful memory needs clear boundaries — separate investigations may require separate conversation contexts to prevent evidence from crossing case boundaries.
