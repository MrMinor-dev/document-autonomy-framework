# Sample Strategic Document Template

This is a copy-paste template showing the complete hybrid documentation format.

---

## [Document Title]

**Overview:**
[1-2 paragraphs explaining what this document covers and why it matters]

---

## 🎯 Decision Authority

**Level 1 - Execute Immediately (No approval needed):**
- [Action 1 that AI can do autonomously]
- [Action 2 that AI can do autonomously]
- [Action 3 that AI can do autonomously]

**Level 2 - Propose and Wait (Requires approval):**
- [Action requiring human review]
- [Action requiring human review]
- [Action requiring human review]

**Level 3 - Flag for Discussion (Strategic decision):**
- [Major decision requiring CEO/executive]
- [Major decision requiring CEO/executive]
- [Major decision requiring CEO/executive]

---

## ⚠️ Risk Assessment

**Low Risk Scenarios:**
- [Situation 1]: Probability [low/medium/high]
  - **Impact:** [description]
  - **Mitigation:** [how to prevent/reduce]

**Medium Risk Scenarios:**
- [Situation 2]: Probability [low/medium/high]
  - **Impact:** [description]
  - **Mitigation:** [how to prevent/reduce]

**High Risk Scenarios:**
- [Situation 3]: Probability [low/medium/high]
  - **Impact:** [description]
  - **Mitigation:** [how to prevent/reduce]
  - **Escalation:** Required immediately

---

## 📊 Success Metrics

**Primary KPIs:**
- **[Metric Name 1]:** Target [value] [unit]
- **[Metric Name 2]:** Target [value] [unit]
- **[Metric Name 3]:** Target [value] [unit]

**How to measure:**
- [Tracking method]
- [Update frequency: daily/weekly/monthly]

**Thresholds:**
- 🟢 Green: >[target value]
- 🟡 Yellow: [80-100% of target]
- 🔴 Red: <[80% of target] → Escalate

---

## 🚨 Escalation Protocol

**When to escalate:**
- [Specific trigger 1]
- [Specific trigger 2]
- [Specific trigger 3]

**How to escalate:**
1. Flag immediately (do not proceed)
2. Provide context: [what information to include]
3. Recommend action: [your suggested approach]
4. Wait for human approval

**Response time expectations:**
- Critical issues: [timeframe]
- Important decisions: [timeframe]
- Strategic questions: [timeframe]

---

## 🔄 Update Protocol

**Update triggers:**
- [Condition that requires doc update]
- [Condition that requires doc update]
- [Scheduled review: frequency]

**Related documents:**
- See: [document-name.md] ([relationship description])
- See: [document-name.md] ([relationship description])
- See: [document-name.md] ([relationship description])

**Version history:**
- v1.0 ([date]): Initial version
- v1.1 ([date]): [what changed]

---

<!-- MACHINE-READABLE DECISION RULES -->
<decision_matrix>
  <autonomy_level level="1" description="Autonomous execution">
    <threshold type="spending" max="0" currency="USD"/>
    <threshold type="time" max="30" unit="minutes"/>
    <action>execute_immediately</action>
    <requires_approval>false</requires_approval>
    <examples>
      <example>[Specific example of Level 1 action]</example>
      <example>[Another Level 1 example]</example>
    </examples>
  </autonomy_level>
  
  <autonomy_level level="2" description="Manager approval required">
    <threshold type="spending" min="1" max="1000" currency="USD"/>
    <action>propose_and_wait</action>
    <requires_approval>true</requires_approval>
    <approver role="manager"/>
    <response_time max="48" unit="hours"/>
    <examples>
      <example>[Specific example of Level 2 action]</example>
      <example>[Another Level 2 example]</example>
    </examples>
  </autonomy_level>
  
  <autonomy_level level="3" description="Executive approval required">
    <threshold type="spending" min="1001" currency="USD"/>
    <action>flag_for_discussion</action>
    <escalation_required>true</escalation_required>
    <approver role="ceo"/>
    <response_time max="7" unit="days"/>
    <examples>
      <example>[Specific example of Level 3 action]</example>
      <example>[Another Level 3 example]</example>
    </examples>
  </autonomy_level>
</decision_matrix>

<risk_assessment>
  <scenario type="[scenario_name]" likelihood="medium">
    <trigger>[What causes this risk]</trigger>
    <probability>0.15</probability>
    <impact 
      severity="medium" 
      financial_cost="1000-5000"
      operational_impact="[description]"/>
    <mitigation>[How to prevent or reduce this risk]</mitigation>
    <escalation_required>true</escalation_required>
    <notify>ceo,manager</notify>
  </scenario>
</risk_assessment>

<success_metrics>
  <kpi name="[metric_name]" type="financial">
    <target>5000</target>
    <unit>USD</unit>
    <frequency>monthly</frequency>
    <threshold_warning>4000</threshold_warning>
    <threshold_critical>3000</threshold_critical>
    <measurement_method>[How to calculate this metric]</measurement_method>
  </kpi>
</success_metrics>

<escalation_protocol>
  <trigger condition="[condition_name]" value="1000" unit="USD" operator="exceeds">
    <action>notify_immediately</action>
    <notify>ceo</notify>
    <method>email</method>
    <method>sms</method>
    <urgency>high</urgency>
    <include_context>
      <context_item>[data_field_1]</context_item>
      <context_item>[data_field_2]</context_item>
    </include_context>
  </trigger>
</escalation_protocol>
