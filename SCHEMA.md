# XML Schema Documentation

Complete reference for machine-readable XML sections in the Document Autonomy System.

---

## 📋 Table of Contents

1. [Core Schemas](#core-schemas)
2. [Decision Matrix Schema](#decision-matrix-schema)
3. [Risk Assessment Schema](#risk-assessment-schema)
4. [Success Metrics Schema](#success-metrics-schema)
5. [Escalation Protocol Schema](#escalation-protocol-schema)
6. [Automation Rules Schema](#automation-rules-schema)
7. [Validation Rules](#validation-rules)

---

## Core Schemas

### Decision Matrix

**Purpose:** Define autonomy levels with specific thresholds and actions.

**Schema:**
```xml
<decision_matrix>
  <autonomy_level level="[1|2|3]" description="[optional text]">
    <threshold 
      type="[spending|time|quality_score|count]" 
      min="[number]" 
      max="[number]" 
      currency="[USD|EUR|...]"
      unit="[minutes|hours|days|percent|items]"/>
    
    <action>[execute_immediately|propose_and_wait|flag_for_discussion]</action>
    <requires_approval>[true|false]</requires_approval>
    <approver role="[ceo|manager|technical_lead]"/>
    <response_time max="[number]" unit="[minutes|hours|days]"/>
    
    <examples>
      <example>[Specific example of this level]</example>
    </examples>
  </autonomy_level>
</decision_matrix>
```

**Attributes:**
- `level`: Integer 1, 2, or 3
- `type`: Category of threshold
- `min/max`: Numeric bounds (inclusive)
- `currency`: ISO currency code
- `unit`: Measurement unit

**Example:**
```xml
<decision_matrix>
  <autonomy_level level="2" description="Manager approval required">
    <threshold type="spending" min="1" max="1000" currency="USD"/>
    <action>propose_and_wait</action>
    <requires_approval>true</requires_approval>
    <approver role="manager"/>
    <response_time max="48" unit="hours"/>
    <examples>
      <example>$250 software subscription</example>
      <example>$500 conference ticket</example>
    </examples>
  </autonomy_level>
</decision_matrix>
```

---

### Risk Assessment Schema

**Purpose:** Define risk scenarios with probability, impact, and mitigation.

**Schema:**
```xml
<risk_assessment>
  <scenario 
    type="[descriptive_name]" 
    likelihood="[low|medium|high]">
    
    <trigger>[Specific condition that triggers this risk]</trigger>
    <probability>[0.0-1.0]</probability>
    
    <impact 
      severity="[low|medium|high|critical]" 
      financial_cost="[number]"
      operational_impact="[description]"
      legal_risk="[low|medium|high]"/>
    
    <mitigation>[How to prevent or reduce this risk]</mitigation>
    <escalation_required>[true|false]</escalation_required>
    <notify>[role,role,...]</notify>
  </scenario>
</risk_assessment>
```

**Attributes:**
- `likelihood`: Probability category
- `severity`: Impact level
- `financial_cost`: Dollar amount or range
- `probability`: Decimal 0.0-1.0

**Example:**
```xml
<risk_assessment>
  <scenario type="budget_overrun" likelihood="medium">
    <trigger>Monthly expenses exceed budget by >20%</trigger>
    <probability>0.15</probability>
    <impact 
      severity="medium" 
      financial_cost="1000-5000"
      operational_impact="Project delays"/>
    <mitigation>Weekly budget reviews, alerts at 80%</mitigation>
    <escalation_required>true</escalation_required>
    <notify>ceo,finance</notify>
  </scenario>
</risk_assessment>
```

---

### Success Metrics Schema

**Purpose:** Define measurable KPIs with targets and thresholds.

**Schema:**
```xml
<success_metrics>
  <kpi 
    name="[metric_name]" 
    type="[financial|percentage|count|duration]">
    
    <target>[numeric_value]</target>
    <unit>[USD|percent|items|days|hours]</unit>
    <frequency>[daily|weekly|monthly|quarterly|annual]</frequency>
    
    <threshold_warning>[value at which to warn]</threshold_warning>
    <threshold_critical>[value at which to escalate]</threshold_critical>
    
    <measurement_method>[How to calculate this metric]</measurement_method>
  </kpi>
</success_metrics>
```

**Attributes:**
- `type`: Data type of metric
- `target`: Goal value
- `threshold_warning`: Yellow alert level
- `threshold_critical`: Red alert level

**Example:**
```xml
<success_metrics>
  <kpi name="monthly_revenue" type="financial">
    <target>5000</target>
    <unit>USD</unit>
    <frequency>monthly</frequency>
    <threshold_warning>4000</threshold_warning>
    <threshold_critical>3000</threshold_critical>
    <measurement_method>Sum of all revenue transactions for month</measurement_method>
  </kpi>
  
  <kpi name="content_quality_score" type="percentage">
    <target>85</target>
    <unit>percent</unit>
    <frequency>per_item</frequency>
    <threshold_warning>80</threshold_warning>
    <threshold_critical>70</threshold_critical>
    <measurement_method>Weighted average: grammar(25%) + voice(25%) + seo(25%) + format(25%)</measurement_method>
  </kpi>
</success_metrics>
```

---

### Escalation Protocol Schema

**Purpose:** Define triggers and actions for escalation scenarios.

**Schema:**
```xml
<escalation_protocol>
  <trigger 
    condition="[condition_name]" 
    value="[threshold_value]"
    unit="[measurement_unit]"
    operator="[exceeds|below|equals]">
    
    <action>[notify_immediately|halt_operations|flag_for_review]</action>
    <notify>[role,role,...]</notify>
    <method>[sms|email|slack]</method>
    <urgency>[low|medium|high|critical]</urgency>
    
    <include_context>
      <context_item>[data_to_include]</context_item>
    </include_context>
  </trigger>
</escalation_protocol>
```

**Attributes:**
- `condition`: What to check
- `operator`: Comparison type
- `urgency`: Priority level
- `method`: Communication channel

**Example:**
```xml
<escalation_protocol>
  <trigger condition="spending_exceeds" value="1000" unit="USD" operator="exceeds">
    <action>notify_ceo</action>
    <action>require_approval</action>
    <notify>ceo</notify>
    <method>email</method>
    <method>sms</method>
    <urgency>high</urgency>
    <include_context>
      <context_item>expense_amount</context_item>
      <context_item>vendor_name</context_item>
      <context_item>business_justification</context_item>
      <context_item>budget_impact</context_item>
    </include_context>
  </trigger>
  
  <trigger condition="error_rate_above" value="5" unit="percent" operator="exceeds">
    <action>notify_technical_lead</action>
    <action>investigate_immediately</action>
    <notify>technical_lead</notify>
    <method>slack</method>
    <urgency>critical</urgency>
  </trigger>
</escalation_protocol>
```

---

### Automation Rules Schema

**Purpose:** Define automated workflows with triggers and actions.

**Schema:**
```xml
<automation_rules>
  <trigger condition="[event_name]" frequency="[continuous|scheduled]">
    <action>[workflow_to_execute]</action>
    <rate_limit>[max_per_time_period]</rate_limit>
    <timeout max="[number]" unit="[seconds|minutes]"/>
    
    <prerequisites>
      <system>[required_system]</system>
    </prerequisites>
    
    <fallback_procedure>[what_to_do_if_fails]</fallback_procedure>
  </trigger>
</automation_rules>
```

**Attributes:**
- `frequency`: How often to check
- `rate_limit`: Max executions per period
- `timeout`: Max execution time

**Example:**
```xml
<automation_rules>
  <trigger condition="new_file_in_staging" frequency="continuous">
    <action>process_invoice_workflow</action>
    <rate_limit>1_per_second</rate_limit>
    <timeout max="30" unit="seconds"/>
    <prerequisites>
      <system>claude_api</system>
      <system>database</system>
    </prerequisites>
    <fallback_procedure>Move to errors folder, notify admin</fallback_procedure>
  </trigger>
  
  <trigger condition="daily_report_schedule" frequency="scheduled">
    <action>generate_daily_summary</action>
    <schedule>0 9 * * *</schedule>
    <timeout max="5" unit="minutes"/>
  </trigger>
</automation_rules>
```

---

### Failure Thresholds Schema

**Purpose:** Define system health metrics and alert thresholds.

**Schema:**
```xml
<failure_thresholds>
  <metric name="[metric_name]" type="[percentage|count|duration]">
    <warning>[yellow_threshold]</warning>
    <critical>[red_threshold]</critical>
    <unit>[percent|count|milliseconds]</unit>
    <action>[notify_immediately|auto_recover|escalate]</action>
    <notify>[role]</notify>
  </metric>
</failure_thresholds>
```

**Example:**
```xml
<failure_thresholds>
  <metric name="error_rate" type="percentage">
    <warning>5</warning>
    <critical>10</critical>
    <unit>percent</unit>
    <action>notify_immediately</action>
    <notify>technical_lead</notify>
  </metric>
  
  <metric name="response_time" type="duration">
    <warning>1000</warning>
    <critical>3000</critical>
    <unit>milliseconds</unit>
    <action>investigate_performance</action>
    <notify>technical_lead</notify>
  </metric>
</failure_thresholds>
```

---

### Content Quality Schema

**Purpose:** Define content scoring with weighted components.

**Schema:**
```xml
<content_quality_matrix>
  <quality_score minimum="[min_score]" maximum="[max_score]">
    <component name="[component_name]" weight="[points]">
      <criteria>[what_to_check]</criteria>
      <perfect_score>[value_for_perfect]</perfect_score>
      <acceptable_threshold>[minimum_acceptable]</acceptable_threshold>
      <red_flags>[what_indicates_failure]</red_flags>
    </component>
  </quality_score>
</content_quality_matrix>
```

**Example:**
```xml
<content_quality_matrix>
  <quality_score minimum="80" maximum="100">
    <component name="grammar_spelling" weight="25">
      <criteria>Zero spelling or grammar errors</criteria>
      <perfect_score>0</perfect_score>
      <acceptable_threshold>2</acceptable_threshold>
      <red_flags>More than 5 errors</red_flags>
    </component>
    
    <component name="brand_voice" weight="25">
      <criteria>Honest, data-driven, helpful tone</criteria>
      <red_flags>Hyperbolic, salesy, vague language</red_flags>
    </component>
    
    <component name="seo_optimization" weight="25">
      <keyword_density min="0.5" max="2.5" unit="percent"/>
      <meta_description required="true" max_length="160"/>
      <heading_structure required="H1,H2,H3"/>
    </component>
  </quality_score>
</content_quality_matrix>
```

---

### Partnership Scoring Schema

**Purpose:** Define multi-factor scoring matrices for evaluation.

**Schema:**
```xml
<partnership_scoring_matrix total_points="[max_points]">
  <category name="[category_name]" max_points="[points]">
    <tier score="[points]" [optional_attributes]>[description]</tier>
  </category>
  
  <decision_thresholds>
    <threshold score_min="[min]" score_max="[max]">
      <recommendation>[decline|evaluate|approve|prioritize]</recommendation>
      <action>[auto_action]</action>
      <priority>[low|medium|high]</priority>
      <rationale>[reasoning]</rationale>
    </threshold>
  </decision_thresholds>
</partnership_scoring_matrix>
```

**Example:**
```xml
<partnership_scoring_matrix total_points="40">
  <category name="brand_reputation" max_points="10">
    <tier score="10">Household name brand</tier>
    <tier score="7">Well-known in niche</tier>
    <tier score="5">Emerging brand with traction</tier>
    <tier score="2">Unknown brand</tier>
  </category>
  
  <category name="revenue_potential" max_points="10">
    <tier score="10" annual_revenue_min="10000"/>
    <tier score="7" annual_revenue_min="5000" annual_revenue_max="9999"/>
    <tier score="5" annual_revenue_min="1000" annual_revenue_max="4999"/>
    <tier score="2" annual_revenue_max="999"/>
  </category>
  
  <decision_thresholds>
    <threshold score_min="0" score_max="24">
      <recommendation>decline</recommendation>
      <action>auto_decline_email</action>
      <priority>low</priority>
      <rationale>Low value, poor fit</rationale>
    </threshold>
    
    <threshold score_min="36" score_max="40">
      <recommendation>prioritize</recommendation>
      <action>flag_for_immediate_discussion</action>
      <priority>high</priority>
      <rationale>Exceptional opportunity, top 10%</rationale>
    </threshold>
  </decision_thresholds>
</partnership_scoring_matrix>
```

---

## Validation Rules

### Required Attributes

**Decision Matrix:**
- `level` must be 1, 2, or 3
- `threshold` must have `type` attribute
- Numeric `min/max` must be present if applicable
- `action` must be valid enum value

**Risk Assessment:**
- `likelihood` must be low|medium|high
- `severity` must be low|medium|high|critical
- `probability` must be 0.0-1.0 if present

**Success Metrics:**
- `target` must be numeric
- `unit` must be present
- `frequency` must be valid time period

### Data Type Validation

**Numeric Fields:**
```python
# Must be valid numbers
threshold.min >= 0
threshold.max >= threshold.min
probability >= 0.0 and probability <= 1.0
```

**Enum Fields:**
```python
# Must match allowed values
action in ['execute_immediately', 'propose_and_wait', 'flag_for_discussion']
severity in ['low', 'medium', 'high', 'critical']
frequency in ['daily', 'weekly', 'monthly', 'quarterly', 'annual']
```

**Required Fields:**
```python
# Cannot be empty
<threshold type="spending" ...>  # type is required
<action>...</action>  # must have text content
<target>5000</target>  # must have numeric value
```

### XML Well-Formedness

**Valid XML:**
```xml
<!-- ✅ Properly nested -->
<decision_matrix>
  <autonomy_level level="1">
    <threshold type="spending" max="0"/>
  </autonomy_level>
</decision_matrix>
```

**Invalid XML:**
```xml
<!-- ❌ Not properly closed -->
<decision_matrix>
  <autonomy_level level="1">
    <threshold type="spending" max="0">
  </autonomy_level>
```

### Validation Script

**Python Example:**
```python
import xml.etree.ElementTree as ET

def validate_decision_matrix(xml_string):
    try:
        root = ET.fromstring(xml_string)
        
        # Check for required elements
        for level in root.findall('.//autonomy_level'):
            assert level.get('level') in ['1', '2', '3'], "Invalid level"
            
            threshold = level.find('threshold')
            assert threshold is not None, "Missing threshold"
            assert threshold.get('type'), "Missing threshold type"
            
            action = level.find('action')
            assert action is not None, "Missing action"
            assert action.text in [
                'execute_immediately',
                'propose_and_wait',
                'flag_for_discussion'
            ], "Invalid action"
            
        return True, "Valid"
    except (ET.ParseError, AssertionError) as e:
        return False, str(e)

# Usage
valid, message = validate_decision_matrix(xml_content)
if valid:
    print("✅ XML is valid")
else:
    print(f"❌ Validation failed: {message}")
```

---

## Common Patterns

### Pattern 1: Spending Thresholds

```xml
<!-- $0 = Level 1, $1-1000 = Level 2, >$1000 = Level 3 -->
<decision_matrix>
  <autonomy_level level="1">
    <threshold type="spending" max="0" currency="USD"/>
    <action>execute_immediately</action>
  </autonomy_level>
  <autonomy_level level="2">
    <threshold type="spending" min="1" max="1000" currency="USD"/>
    <action>propose_and_wait</action>
  </autonomy_level>
  <autonomy_level level="3">
    <threshold type="spending" min="1001" currency="USD"/>
    <action>flag_for_discussion</action>
  </autonomy_level>
</decision_matrix>
```

### Pattern 2: Time-Based Limits

```xml
<!-- Max time constraints per autonomy level -->
<decision_matrix>
  <autonomy_level level="1">
    <threshold type="time" max="30" unit="minutes"/>
    <action>execute_immediately</action>
  </autonomy_level>
  <autonomy_level level="2">
    <threshold type="time" min="31" max="240" unit="minutes"/>
    <action>propose_and_wait</action>
  </autonomy_level>
</decision_matrix>
```

### Pattern 3: Quality Score Gating

```xml
<!-- Minimum quality score to proceed -->
<content_quality_matrix>
  <quality_score minimum="80" maximum="100">
    <component name="grammar" weight="25"/>
    <component name="brand_voice" weight="25"/>
    <component name="seo" weight="25"/>
    <component name="formatting" weight="25"/>
  </quality_score>
  
  <publication_rules>
    <threshold score_min="80">
      <action>propose_publication</action>
      <requires_approval>true</requires_approval>
    </threshold>
    <threshold score_max="79">
      <action>flag_for_revision</action>
      <escalation_required>true</escalation_required>
    </threshold>
  </publication_rules>
</content_quality_matrix>
```

### Pattern 4: Multi-Factor Scoring

```xml
<!-- Evaluate on multiple weighted dimensions -->
<scoring_matrix total_points="40">
  <category name="factor_1" max_points="10">
    <tier score="10">Excellent</tier>
    <tier score="5">Good</tier>
    <tier score="2">Poor</tier>
  </category>
  <category name="factor_2" max_points="10">
    <!-- ... -->
  </category>
  
  <decision_thresholds>
    <threshold score_min="30">
      <recommendation>approve</recommendation>
    </threshold>
    <threshold score_max="29">
      <recommendation>decline</recommendation>
    </threshold>
  </decision_thresholds>
</scoring_matrix>
```

---

## Extension Guidelines

### Adding New Schemas

**When to create new schema:**
- New decision type not covered by existing schemas
- Unique business logic requiring programmatic evaluation
- Complex multi-step workflows needing orchestration

**Schema design principles:**
1. **Keep it simple:** Start minimal, extend as needed
2. **Be explicit:** No implicit defaults, everything specified
3. **Include units:** Never bare numbers (always USD, percent, etc)
4. **Validate:** Provide validation rules and scripts
5. **Document:** Examples for every element

### Custom Schema Template

```xml
<[schema_name]>
  <!-- Overview comment explaining purpose -->
  
  <[element_name] 
    required_attribute="[value]"
    optional_attribute="[value]">
    
    <[subelement]>[content]</[subelement]>
    
    <!-- Nested structures as needed -->
  </[element_name]>
  
  <!-- Validation rules in comments -->
  <!-- required_attribute must be: [constraints] -->
  <!-- optional_attribute defaults to: [default] -->
</[schema_name]>
```

---

**This schema reference enables programmatic parsing and validation of all autonomy rules, making AI decision-making consistent, auditable, and enforceable across the entire organization.**
