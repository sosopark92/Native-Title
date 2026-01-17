## Native Title System Overview

**Applications (in progress) → Registers (officially recorded) → Determinations (final outcomes)**

Alongside that: **ILUAs** (agreements) and **Future Acts** (activities that may affect native title)

</aside>

According to the [NNTT Statistics](https://www.nntt.gov.au/Pages/Statistics.aspx) page, the native title system can be summarised in three key sections: 

- **Current Applications** covers matters that are currently in progress (including native title applications, plus separate counts for future act applications and ILUAs lodged)
    - **Native title applications**
    - Future act applications
    - Indigenous Land Use Agreements (ILUAs) lodged
- **Native Title Registers** covers matters that have reached an official registered status (such as claims on the Register of Native Title Claims, ILUAs on the Register of ILUAs, and determinations recorded on the National Native Title Register)
    - Register of Native Title Claims (RNTC)
    - Register of Indigenous Land Use Agreements (ILUAs)
    - National Native Title Register (NNTR)
- **Native Title Determinations** reports outcomes where a court or recognised body has determined whether native title exists or not.
    - Native title determinations made by a court or other recognised body
    - Existence of native title

*→ For this project, I used the **Schedule of Native Title Determination Applications** dataset because it provides the case-level detail behind the **“Current Applications”** view, making it possible to build a clearer, more interpretable summary.*

## Dataset
<aside>
**Schedule of Native Title Determination Applications (***Date Extracted: 14-01-2026)*
https://digital.atlas.gov.au/datasets/digitalatlas::schedule-of-native-title-determination-applications/about
</aside>

***Schedule of Native Title Determination Applications*** dataset contains current information about **all native title determination applications** (claimant, non-claimant, compensation, revised determination) that the Federal Court provides to the Registrar. 

- **Columns**
    
    ```markdown
    **Key fields used in this analysis:**
    - Identifiers: `Tribunal No`, `Fed Court No`, `Name`, `NNTT_Seq_No`
    - Categorisation: `Type`, `Status`, `Jurisdiction`
    - Registration milestones: `Reg Test Status`, `Reg Test Decision`, `Date Registered`
    - Complexity proxies: `Sea Claim`, `Overlap`
    - Size: `Area sq km`
    - Freshness: `Date_Currency`, `Date_extracted`
    ```
    
    - **Tribunal No**: NNTT/Tribunal matter identifier
    - **NNTT_Seq_No**: Internal sequence/unique ID for the record
    - **Fed Court No**: Federal Court file/reference number
    - **Name**: Application name
    - **Type**: Application type (`Claimant`/ `Non-claimant` / `Compensation`/ `Revised Native Title Determination`)
    - **Status**: Operational status (`Active`/`Full Approved Determination`)
    - **Jurisdiction**: State/Territory jurisdiction (`QLD`, `NT`, `NSW`, `WA`, `VIC`, `SA`)
    - **Reg Test Status**: Where the claim sits in registration testing
        
        ```
        Accepted for registration
        Not currently identified for Reg. Decision
        Not Accepted for registration
        Currently identified for Reg. Decision (new decision in progress - s 190A)
        Accepted for registration (new decision in progress - s 190A)
        Not Accepted for registration (new decision in progress - s 190A)
        Not Accepted for registration (new decision in progress - s 190E reconsideration)
        ```
        
    - **Reg Test Decision**: Date of registration test decision
    - **Date Registered**: Date the claim was accepted and entered on the Register (if accepted)
    - **Area sq km**: Area of the claim footprint
    - **Sea Claim**: Whether it includes sea/offshore areas
    - **Overlap**: Whether the geometry overlaps with other matters
    - **Lodged**: Date filed/lodged
    - **Date_Currency**
    - **Date_extracted**
    - **Combined**
    - **Representative**
    
    **GIS geometry fields**
    
    - **Shape__Area / Shape__Length**: Geometry calculations from the **shapefile**
 
  ## Analysis Questions
  
1. **What is the current caseload?**
2. **How far have matters progressed through key milestones?** 
    : Lodged → Reg Test Decision → Date Registered
    
3. **How long have matters been in the system?**  
    : Processing age distribution + Stages 
4. **Are there visible patterns by jurisdiction, sea claim, overlap, or claim size?**

## Method

- **Data Audit**: Data types, missingness, duplicates, and basic sanity checks.
- **Feature engineering:**
    - Converted milestone dates into a comparable timeline and created a simple `stage reached` indicator.
    - Created `processing age (days)` and `age buckets` (<1 year, 1–3, 3–5, 5–10, 10+ years).
    - Created a conservative `complexity flag` using `Overlap` (Overlapping vs Not recorded).
- **EDA + visualisation:** Bar charts for distributions, milestone reachability, processing-time histogram, comparisons by jurisdiction and sea claim, and a map view for spatial context.
- **Skills:** Python(Pandas, Matplotlib, Writing functions, statistics), PowerBI: Dax, writing functions, calculated columns, data visualisation

## Key results

- Case pipeline visibility: the caseload is not at a single stage. Many matters are lodged but have not progressed to recorded milestones (only ~64% with a reg-test decision date, ~51% with a date registered).
- Wide variation in time-in-system: matters of very different ages coexist under “Active”, from under a month to decades, which is important context for workload interpretation.
- Geographic concentration:* QLD, NSW and NT account for a large share of active matters in this snapshot, which is useful for resourcing and reporting narratives.
- Complexity signals (with limitations): sea claims tend to be older; overlap information is mostly missing and should be treated cautiously.

