# GramRoute AI — Master Upgrade & Build Prompt
## ROUTELESS: The Last Mile | Emergency Examination-Day Challenge

You are working as a senior full-stack engineer, transportation-optimization engineer, and product designer. Your task is to turn **ROUTELESS** into **GramRoute AI**, a credible, explainable rural student-transportation platform.

Build on the project that already exists. The goal is not to throw it away and create a disconnected demo. The goal is to understand the current application, preserve what works, and add the emergency-response capabilities in a way that fits its existing architecture.

The product should feel like software that could realistically help village coordinators manage transport disruptions—not a collection of attractive but meaningless dashboard cards.

Use realistic Karnataka-inspired village and college data, Indian currency (₹), practical transport constraints, and clear explanations. Prioritize correctness, student safety, reproducibility, and a convincing end-to-end demonstration.

---

# 1. Start by understanding the existing project

Before changing anything:

1. Inspect the repository, folder structure, package files, existing screens, components, data models, backend, database, and current workflows.
2. Identify the existing framework, language, styling system, state management, APIs, and deployment setup.
3. Make a short internal plan for integrating the new capabilities into the current application.
4. Preserve existing working features, visual identity, architecture, and technology choices wherever practical.
5. Reuse the current backend and database if they exist. Do not replace them with local mock data simply because that is easier.
6. Do not rebuild the project from scratch, overwrite working functionality, or introduce a second competing architecture.

**Compatibility rule:** The existing project is the source of truth. If it already uses a different stack from the suggested reference stack below, keep the existing stack and adapt the implementation. Only use the reference stack for genuinely missing parts or if the repository is effectively empty.

If the repository is empty or has no usable application, use this reference stack:

- Vite
- React 18
- TypeScript in strict mode
- Tailwind CSS v3
- lucide-react
- Vitest
- React `useReducer` for application state
- Native SVG for maps and charts

For a new standalone implementation, keep it deployable to Vercel or Netlify, avoid paid APIs and API keys, avoid unnecessary dependencies, and pin exact dependency versions. Include SPA rewrite configuration where needed.

Do not assume that a feature exists just because its UI is visible. Trace each important action through its actual logic and state changes.

---

# 2. Product vision

GramRoute AI coordinates rural student transportation through three connected layers:

1. **Demand:** Identify students who need transport, where they are waiting, where they need to go, and when they must arrive.
2. **Optimization:** Allocate available buses and suitable community vehicles, group compatible students, and find feasible routes.
3. **Execution:** Help coordinators, students, guardians, and drivers act on the plan through clear assignments, consent steps, alerts, and trip updates.

The platform should support:

- Government-bus capacity monitoring.
- Student-demand clustering and demand forecasting.
- Dynamic bus and vehicle reallocation.
- Alternative shared-vehicle matching.
- Route optimization and road-closure handling.
- Exam-time-based student prioritization.
- Guardian consent and trip-safety workflows.
- Feature-phone SMS simulation.
- Explainable recommendations and decision logs.
- What-if fleet simulation.
- Simulated backtesting and impact reporting.

Do not describe deterministic rules or simulated data as a trained AI model. Be precise: the prototype's intelligence may use optimization algorithms, graph routing, seeded simulations, and statistical forecasting. Clearly distinguish working functionality from simulated integrations.

---

# 3. Main emergency scenario: examination-day transport crisis

The application must include a repeatable emergency scenario with these conditions:

- Two government buses become unavailable because of technical failures.
- Student transportation demand rises by **40%**.
- Flooding blocks a major road connecting two villages.
- Several students have examinations beginning within the next **two hours**.

The primary objective is:

> Minimize the number of students who miss their examinations while making the safest and most efficient use of the transportation resources that remain.

This is not simply a normal route-planning problem. The system must make difficult resource-allocation decisions, explain who can be transported, identify who cannot, and avoid pretending that every student can be served.

## 3.1 Dynamic bus reallocation

When the emergency is activated:

- Mark the two affected government buses unavailable.
- Recalculate the active fleet and each available bus's remaining passenger capacity.
- Recalculate capacity after existing passengers and confirmed reservations are accounted for.
- Identify routes that have lost service or become overcrowded.
- Reallocate available buses according to demand, route feasibility, examination urgency, and capacity.
- Consider additional trips or revised service frequency only when the schedule and travel times make them feasible.
- Never assign more students than the vehicle can carry.

Show the reason for each reassignment.

## 3.2 Flooded-road handling and alternative routes

- Mark the affected road edge as closed.
- Remove that edge from the active routing graph; do not merely draw a warning over it.
- Recalculate routes using only open roads.
- Show the alternative path and updated travel-time estimate.
- Account for monsoon-related travel-time increases where configured.
- Never recommend a route that includes the flooded or otherwise closed road.
- If no feasible path exists, say so clearly and explain which connection is unavailable.

## 3.3 Examination-based prioritization

Prioritize students using actual trip and exam information, including:

- Examination start time.
- Pickup location.
- Destination.
- Estimated pickup time.
- Estimated arrival time.
- Expected travel duration and disruption delays.

The system should estimate whether each student can reach the examination destination before the exam begins. Students with the least time remaining should receive urgent attention, but a priority score must not override vehicle capacity, road safety, or route feasibility.

Display clear statuses such as:

- **On track:** estimated to arrive before the exam.
- **At risk:** little time remains or the route has significant uncertainty.
- **Likely delayed:** current plan estimates arrival after the exam starts.
- **Unassigned:** no feasible transport has been allocated.

Explain the reason behind each status. Do not promise an arrival time that the route engine has not calculated.

## 3.4 Demand surge and overcrowding

- Apply the 40% demand increase to the baseline demand using a consistent, visible calculation.
- Identify routes and stops where demand exceeds available seats.
- Distribute students across feasible buses and community vehicles.
- Avoid double-counting students across clusters, buses, or dispatches.
- Show baseline demand, increased demand, available seats, allocated students, and remaining unmet demand.

## 3.5 Live emergency updates

When the emergency plan changes, update the actual application state and all relevant views:

- Bus availability and assignments.
- Passenger allocations and remaining seats.
- Route paths and travel-time estimates.
- Student priority and arrival status.
- Emergency alerts and affected villages.
- Unserved-student counts and recommendations.
- Decision history.

Do not animate or change dashboard numbers independently of the underlying data.

## 3.6 Students who cannot be served

The system must explicitly identify students who remain unassigned.

For each unserved group, show:

- Number of students affected.
- Pickup village and destination.
- Reason no assignment was possible.
- Capacity shortage, if applicable.
- Whether the exam can still be reached under the current estimated travel time.
- Practical next steps.

Possible recommendations include:

- An additional trip by an available bus, if timing permits.
- A nearby available and appropriately verified community vehicle.
- A safe alternative pickup point.
- A revised pickup sequence.
- Escalation to a human coordinator for additional transport.

Do not invent vehicles, seats, routes, or successful journeys. If resources are insufficient, report the shortage honestly.

---

# 4. Emergency dashboard requirements

Upgrade the existing ROUTELESS dashboard rather than creating an unrelated screen. The emergency control-centre view should make the situation understandable at a glance.

Display:

- Overall emergency status and activation time.
- Affected villages and the flooded/closed road.
- Total buses, available buses, and unavailable buses.
- Remaining passenger capacity versus total student demand.
- Baseline demand and the 40% demand increase.
- Updated bus assignments and alternative routes.
- Students prioritized for upcoming examinations.
- Exam start times and estimated arrival times.
- Students on track, at risk, likely delayed, and unassigned.
- Overcrowded routes and stops.
- Students still requiring transportation.
- Recommended next actions and the reason for each recommendation.

Use clear visual hierarchy. Critical warnings should stand out, but avoid a cluttered wall of cards.

---

# 5. Domain model and demo data

Use strongly typed models appropriate to the existing codebase. At minimum, represent the concepts below:

- Village stop and college/destination.
- Road edge and road availability.
- Bus route and government bus.
- Student transport request.
- Student cluster.
- Community vehicle and driver.
- Exam schedule and student priority.
- Allocation plan and route-reallocation plan.
- Dispatch and dispatch status.
- Decision-log entry.
- Demand forecast and impact metrics.
- Emergency scenario configuration.

Every entity must have a stable unique ID.

Use fictional, anonymized student IDs. Do not use real student personal information.

For a Karnataka-inspired demonstration, include locations such as:

- Bantwal
- Adyar
- Kothur
- Farangipete
- Vittla
- Mudipu
- Melkar
- BC Road

Seed the demo with at least:

- 8 village stops.
- 3 government bus routes.
- 4 community vehicles.
- 2 regional colleges or examination destinations.
- Realistic student demand and examination schedules.
- Vehicle seat capacities, availability, fares in rupees, and estimated travel times.
- Driver ratings and clearly described verification tiers.

Suggested driver verification tiers:

- ID-verified
- Depot-registered
- Community-vouched
- Unverified

Do not claim Aadhaar verification or police verification unless a real, verified integration exists.

Include a named demo persona:

**Meena — a student waiting at Adyar.**

Use anonymized IDs for all other students. Give Meena a clear examination start time and a journey that can be followed through the demo.

Keep all seeded data deterministic so that resetting the demo produces the same initial scenario.

---

# 6. Road graph and route optimization

Represent village stops and colleges as nodes in a road graph. Each road edge should include:

- Distance in kilometres.
- Road type.
- Base travel time.
- Travel-time multiplier.
- Availability status.

Use Dijkstra's shortest-path algorithm or another appropriate, documented shortest-path algorithm. Calculate ETA from actual graph-edge travel times, not straight-line distance.

Support:

- Road closures and reopening.
- Monsoon travel-time increases.
- Dynamic path recalculation.
- Emergency stop insertion.
- Stop reordering.
- Route reachability checks.

When a road closes, remove it from the active routing graph. If no feasible path exists, return a clear explanation instead of an invented route.

Draw the road network using an interactive SVG map. Make closed roads visually distinct and provide an accessible text/list alternative.

All route and allocation decisions should live in testable service functions, not inside UI components. Pass the current simulated time explicitly to time-dependent functions. Avoid hidden clocks and hidden mutable state inside optimization services.

---

# 7. Allocation and prioritization engine

Implement actual, deterministic allocation logic. Do not use static dashboard values or pretend that a recommendation is feasible without checking it.

The allocation engine must consider:

- Vehicle seat capacity and current occupancy.
- Vehicle availability.
- Route reachability after closures.
- Pickup and destination compatibility.
- Estimated travel time.
- Examination start time and urgency.
- Maximum acceptable detour.
- Driver verification and student-safety rules.
- Existing reservations and dispatches.

A student cluster should normally be assigned as a group unless the data explicitly allows splitting. Use a capacity-constrained allocation method, such as deterministic min-cost flow or a documented constrained search/greedy approach suitable for the size of the dataset.

A vehicle may serve multiple clusters only if its remaining capacity, route, and timing allow it. Never overbook a vehicle.

Never assign a lone minor to a carpool with an unverified driver. Enforce the safety restrictions in code, not just in explanatory text.

If no feasible assignment exists, return an explicit unserved result with human-readable reasoning.

## 7.1 Exam priority

Use a transparent priority calculation based on the time remaining until the exam and the calculated journey. At minimum, compare:

- Current time.
- Estimated pickup delay.
- Travel time after route disruption.
- A reasonable arrival buffer.
- Exam start time.

A student whose estimated arrival is after the exam begins must be marked as at risk or likely delayed, not as successfully served.

Use the priority score to guide allocation among feasible options. It must never bypass safety, capacity, or route constraints.

## 7.2 Recommendation scoring

If recommendations are ranked, normalize every scoring component to 0–1, with higher always meaning better.

Suggested components:

- Cost utility: lower cost scores higher.
- ETA utility: shorter ETA scores higher.
- Reliability: higher reliability scores higher.
- Detour utility: smaller detour scores higher.
- Seat fit: suitable capacity scores higher.
- Verification: stronger verification scores higher.
- Exam feasibility/urgency: prioritize students with less time before their examination.

Keep weights in a separate configuration file and ensure the weights sum to 1. Document the scoring formula and assumptions.

Return ranked feasible recommendations and explain why the selected option is preferable. Generate explanations from actual calculated values; never invent fares, ETAs, detours, confidence values, or savings.

Each allocation result should include, where relevant:

- Cluster/student IDs.
- Vehicle IDs.
- Students covered.
- Total cost and cost per student.
- ETA and detour.
- Driver verification tier.
- Feasibility status.
- Exam-arrival status.
- Reasoning in plain language.

---

# 8. Route and fleet reallocation

The system should recommend useful network changes, not just one-off vehicle matches.

Support recommendations such as:

1. Add a relief bus.
2. Increase service frequency.
3. Insert an emergency feeder stop.
4. Reorder existing stops.
5. Use a feasible additional trip.
6. Move demand to a nearby safe pickup point.

Include a **What-If Fleet Simulator**. It should compare the current plan with a proposed change and show:

- Existing and projected capacity.
- Student demand.
- Risk before and after the change.
- Students who could newly be served.
- Estimated travel-time effect.
- Reasoning and any new constraints.

A what-if simulation must not mutate the live application state. Any example improvement shown in the interface must come from the actual calculation, not a hardcoded claim.

---

# 9. Demand forecasting and simulated backtesting

If the existing project already has forecasting or historical-demand features, extend them instead of duplicating them.

For a new implementation, use a deterministic exponential-smoothing forecaster with a default alpha of 0.4 and clearly labelled synthetic historical demand.

Support configurable seasonal/disruption factors such as:

- Examination days.
- Weekly market/santhe days.
- Monsoon disruptions.
- Festival surges.

Return predicted demand, lower and upper confidence bands (approximately ±15% if appropriate), seasonal factors, and a confidence indicator. Label all synthetic data and forecasts as simulated. Do not claim real-world predictive accuracy.

Implement a 30-day simulated backtest comparing:

- A static government-bus schedule.
- GramRoute AI's dynamic allocation strategy.

Use identical seeded demand and disruption inputs for both approaches. Calculate results from the simulation logic and report:

- Total stranded trips.
- Stranded trips prevented.
- Average waiting time.
- Average waiting time saved.
- Vehicle utilization.
- Estimated student cost savings.

Label all results as simulated. Never hardcode performance claims. Include a compact chart and a summary panel.

---

# 10. Dispatch, consent, and safety workflow

Use a validated dispatch lifecycle. A suggested sequence is:

`requested → matched → guardian_consented → approved → dispatched → completed`

Cancellation is allowed before completion. Validate every transition and reject invalid transitions without corrupting state.

Reservation rules:

- Requested: no seats reserved.
- Matched: seats tentatively reserved.
- Guardian consented: reservation confirmed.
- Approved: coordinator authorizes dispatch.
- Dispatched: vehicle begins the trip.
- Completed: finalize impact metrics.

Recheck vehicle capacity immediately before approval. Use unique dispatch IDs and idempotency checks so approving the same dispatch twice cannot consume seats or update metrics twice.

Cancellation before completion must release reserved seats, restore route occupancy, reopen eligible student requests, remove provisional impact metrics, preserve the decision history, and mark the dispatch cancelled.

A completed dispatch must not be cancelled through ordinary rollback. Handle any correction through a separate administrative process.

Every state transition should append a decision-log entry with the actual reason and calculated values.

Guardian consent must be required wherever the student-safety workflow calls for it. Provide a simulated OTP flow, clearly labelled as a demo. Do not send real SMS or imply that a real guardian has been contacted.

---

# 11. Application state and persistence

Follow the existing application's state-management approach if it is sound. Do not add a second competing state store.

If the project is being built from scratch, React `useReducer` can be the authoritative application-state mechanism.

Persist state using the existing backend/database where available. If the app is intentionally browser-only, use schema-versioned localStorage persistence and document the limitations.

For local persistence:

- Validate the schema version and required fields on load.
- Migrate supported older versions.
- Reset to seeded demo data if saved data is corrupt or unsupported.
- Use unique IDs and prevent duplicate actions.
- Keep state updates deterministic.

If multiple tabs are supported, synchronize carefully using mechanisms such as BroadcastChannel and the browser storage event, with state versions, unique action IDs, stale-update rejection, and a clear single-writer policy for dispatch approvals.

Browser-only synchronization is a demonstration mechanism, not a substitute for server-side transactions. Do not claim that it provides production-grade concurrency guarantees.

---

# 12. Simulation controls

Provide a centralized simulation clock if the existing application needs one.

Suggested demo timing:

- One simulation tick every 3 real seconds.
- Each tick advances 1 simulated minute.
- ETAs decrease consistently with simulated time.
- Vehicles move along active graph edges.
- Stale student requests expire after a configurable period.
- Demand changes use seeded randomness.

Provide controls to:

- Start or pause the simulation.
- Reset the scenario.
- Select a deterministic seed.
- Trigger or clear supported disruptions.

A reset must reproduce the same initial state. Do not create independent clocks in separate components. Respect `prefers-reduced-motion` while keeping functional updates available.

---

# 13. User interface and workflows

Keep the existing ROUTELESS visual identity where practical, but make the experience polished, consistent, responsive, and easy to understand.

Suggested design direction:

- Light neutral backgrounds.
- Deep green as the primary colour.
- High-contrast text.
- Restrained warning and status colours.
- Consistent spacing and cards.
- Lucide icons if compatible with the current stack.
- Responsive desktop and mobile layouts.
- No excessive gradients, generic AI illustrations, or decorative elements that obscure the information.

## 13.1 Header and navigation

Include, where appropriate to the existing application:

- GramRoute AI branding.
- Live system status.
- Student App / Control Center view switch.
- English / Kannada / Hindi language switch.
- Scenario selector.
- Bus Breakdown trigger.
- Exam-Day Demand Surge trigger.
- Heavy Rain / Road Closure trigger.
- Route Cancellation trigger.
- Run Demo button.

Translate primary navigation, alerts, action labels, and SMS replies. Detailed technical explanations may remain in English.

## 13.2 Student view

Provide a mobile-first student experience with:

- Village/pickup selector.
- College/exam destination selector.
- Exam start-time information where relevant.
- Live bus capacity and route status.
- Estimated arrival time.
- A way to report that a bus is full or disrupted.
- Ranked shared-vehicle alternatives.
- Shared fare and ETA.
- Driver-verification information.
- A clear explanation of why an option was recommended.
- Guardian-consent step.
- Trip Safety Code.
- Pickup and drop-off information.
- Teacher or village-coordinator proxy request.

On small screens, use a compact stop list and capacity indicators instead of forcing a large map into the layout.

## 13.3 Guardian consent

- Generate a demo OTP.
- Clearly state that the OTP is simulated.
- Show consent status.
- Prevent dispatch from proceeding until required consent has been completed.
- Do not claim that a real message was sent.

## 13.4 SMS simulator

Provide a feature-phone-inspired SMS simulator, clearly labelled as simulated.

Support:

- Texting `1` to request a last-mile match.
- Returning the matched vehicle, fare, and ETA.
- Generating a four-digit Trip Safety Code.
- Texting `YES` to confirm simulated guardian consent.
- Simulating a panic alert.
- Displaying a trip-safety event log.

Use the active language selection for SMS replies. Do not send real SMS.

## 13.5 Coordinator control centre

Include:

1. Interactive SVG corridor map.
2. Student-demand heat circles.
3. Vehicle positions and availability.
4. Capacity and overcrowding alerts.
5. Road closures and alternative routes.
6. Examination-priority queue.
7. Student arrival-risk statuses.
8. Unserved-student list and shortage summary.
9. Recommendation panel with explanations.
10. Approve Dispatch and Cancel Dispatch actions.
11. Explainable decision log.
12. What-If Fleet Simulator.
13. Demand Forecast.
14. 30-Day Backtest.
15. Impact panel.

Approving a dispatch must update the real application state, including vehicle reservations, route occupancy, student-request statuses, queue counts, dispatch status, and eligible impact metrics.

Do not merely animate dashboard values.

## 13.6 Impact panel

Show metrics such as:

- Students transported successfully.
- Estimated rupees saved.
- Average waiting minutes saved.
- Completed trips.
- Vehicle utilization.
- Students still unserved.

Calculate actual impact from completed demo dispatches. Do not count a merely matched, consented, or approved trip as a completed rescue.

Keep actual simulated results separate from projections. A student who is allocated a vehicle but is still estimated to miss the exam must not be presented as an examination successfully saved.

---

# 14. Guided demonstration

Create a repeatable guided demo centred on **Meena, waiting at Adyar**.

The demo should show a coherent journey from disruption to resolution:

1. Start with the normal seeded scenario.
2. Trigger the monsoon disruption.
3. Make the relevant government bus unavailable.
4. Close the flooded road in the routing graph.
5. Show the affected corridor and villages.
6. Meena submits a request, for example through SMS `1`.
7. The system identifies her request and any compatible student cluster.
8. The optimizer checks available capacity, route feasibility, safety, and exam timing.
9. Show a feasible recommendation—or explain honestly why none exists.
10. Display the fare, ETA, driver verification, and recommendation reasoning.
11. Complete the simulated guardian-consent step.
12. Let the coordinator approve the dispatch.
13. Show the vehicle progressing along the safe SVG route.
14. Update the ETA and trip status.
15. Complete the trip if the simulated journey reaches its destination.
16. Update the impact panel only when completion criteria are met.
17. Record the decisions and transitions in the decision log.

Provide clear step indicators and allow the demo to pause, resume, and reset.

The same seed must reproduce the same scenario. Keep student counts, capacities, routes, ETAs, and impact calculations consistent throughout the demo. Never claim a number of students were rescued unless the allocation and completed-trip data support it.

---

# 15. Accessibility, privacy, and responsible use

Implement:

- Keyboard-navigable interactive map elements.
- ARIA labels for controls.
- Visible keyboard focus.
- At least 4.5:1 text contrast where applicable.
- Large mobile touch targets.
- Reduced-motion support.
- Semantic HTML.
- Accessible form labels.
- Text alternatives for map information.

Use anonymized student identifiers and avoid collecting real student data in the prototype.

Include this privacy note, adapting only if the actual implementation requires it:

> GramRoute AI uses anonymized student identifiers in this demonstration. No real student personal data is collected. This prototype illustrates privacy-conscious design aligned with India's Digital Personal Data Protection framework; it is not a certification of DPDP Act compliance.

Provide a Reset Demo Data action.

---

# 16. Testing and critical invariants

Use the testing framework already present. If starting from scratch, use Vitest.

Write meaningful tests for routing, allocation, dispatch transitions, forecasting, and persistence.

At minimum, verify these invariants:

1. No vehicle assignment exceeds seat capacity.
2. Student clusters remain intact unless splitting is explicitly allowed.
3. Unverified-driver safety restrictions are enforced.
4. Duplicate dispatch approval is idempotent.
5. Invalid state transitions are rejected.
6. Cancellation restores reserved resources exactly once.
7. Increasing demand cannot reduce overcrowding risk when other inputs remain fixed.
8. Forecast output is deterministic for the same seed and input.
9. Backtest output is deterministic for the same seed and input.
10. Reset restores the exact seeded initial state.
11. Closed roads are never used by the routing engine.
12. Two stale dispatch actions cannot overbook a vehicle.
13. Exam-arrival statuses are based on calculated ETAs and exam times.
14. Unserved students are reported rather than silently dropped.
15. The 40% demand increase is applied consistently and does not double-count students.

Do not weaken tests to make an incorrect implementation pass.

For a new standalone project, the expected commands are:

```bash
npm run build
npm test
```

The build should include TypeScript verification, for example:

```bash
tsc --noEmit && vite build
```

Do not report a build or test as successful unless you actually ran it.

---

# 17. Documentation and presentation material

Update or create a useful README that explains:

- Product overview and problem statement.
- Existing project features retained.
- New emergency-response features.
- Architecture and technology stack.
- Project structure.
- Local setup and run instructions.
- Build and test commands.
- Deployment steps for the platforms the project supports.
- A walkthrough of the emergency scenario.
- Demo controls and reset behaviour.
- Known limitations.
- Which integrations and data are simulated versus real.
- Future development roadmap.

Include a concise 60-second judge demonstration script.

Suggested future roadmap:

1. Start with explainable deterministic optimization and synthetic forecasting.
2. Collect anonymized, consent-based real ridership data.
3. Train and validate demand-prediction models.
4. Compare predictions with actual outcomes.
5. Introduce validated models with human oversight.

Suggested pilot:

- Begin with 2–3 villages.
- Partner with local educational institutions.
- Involve village coordinators and appropriately registered drivers.
- Measure waiting time, transport-related missed classes/exams, cost per student, vehicle utilization, completed trips, and safety incidents.

Potential funding partners may include Block Education Offices, Panchayat support, CSR partnerships, and institutional transportation partnerships. Present these as possible future routes, not confirmed funding.

Potential production work includes secure backend synchronization, an offline-capable PWA, real SMS/IVR, local-language voice assistance, real driver verification, secure guardian consent, live vehicle availability, and privacy/security audits. Do not imply these integrations already exist.

---

# 18. Recommended implementation order

Work in a sensible sequence, adapting it to the existing repository:

**Phase 1 — Understand and protect the current app**
- Inspect the project and identify existing working features.
- Map the current architecture and data flow.
- Decide where emergency handling belongs.
- Preserve the existing stack and working workflows.

**Phase 2 — Core emergency logic**
- Update domain types and seed data.
- Implement bus failure and the 40% demand surge.
- Implement the road graph and flooded-road closure.
- Implement safe alternative routing.
- Implement capacity-aware allocation and exam prioritization.
- Identify and explain unserved students.

**Phase 3 — Application workflow**
- Integrate emergency state into the existing UI.
- Add bus reassignment and route updates.
- Add coordinator approval and dispatch lifecycle.
- Add guardian consent and the SMS simulator where appropriate.
- Ensure all visible controls perform real actions.

**Phase 4 — Supporting intelligence and demo**
- Add forecasting and backtesting if time and existing architecture permit.
- Add the what-if simulator.
- Add the guided Meena demo.
- Add persistence and synchronization appropriate to the project.

**Phase 5 — Verification and polish**
- Test routing, capacity, priority, dispatch, and reset invariants.
- Run the build and tests.
- Fix errors rather than hiding them.
- Check responsive layouts and accessibility.
- Update the README and demo script.

Do not stop after producing a plan, architecture diagram, mock components, or static dashboard. Implement the working application. If execution time is limited, prioritize the emergency routing and allocation engine, exam-based prioritization, unserved-student handling, and a complete end-to-end demonstration. Report unfinished work honestly.

---

# 19. Final acceptance criteria

The upgraded project is acceptable only when:

- The existing project has been inspected and its working features preserved.
- The emergency scenario can be activated and reset.
- Two buses become unavailable in the scenario.
- Demand increases by exactly 40% from the defined baseline.
- The flooded road is excluded from route planning.
- Safe alternative routes and calculated ETAs are displayed.
- Bus and vehicle assignments respect actual remaining capacity.
- Students are prioritized using exam start times and calculated arrival times.
- Overcrowded routes and stops are identified.
- Unserved students and the transportation shortage are visible.
- Practical next steps are suggested for unserved students.
- Changes update the actual application state, not just dashboard animations.
- The coordinator can review and approve feasible dispatches.
- Required guardian consent is enforced in the demo workflow.
- The guided Meena scenario works end to end, or clearly explains any infeasible outcome.
- The app does not claim that every student can be transported.
- Closed roads are never included in recommended routes.
- Build and tests have been run, with their actual results reported.
- Documentation explains the real and simulated parts of the system.

At completion, provide a concise report using:

- **DONE**
- **PARTIAL**
- **SKIPPED**

Also report:

- What existing functionality was preserved.
- Main files changed or created.
- Build result.
- Test result.
- Important assumptions.
- Known limitations.
- How to run and demonstrate the application.

Never claim tests passed if they were not executed. Never describe simulated integrations as live services.

---

# Final direction

Build GramRoute AI as a practical, explainable rural mobility system—not a fake-AI showcase.

The defining demonstration is the examination-day crisis: two buses are lost, demand rises by 40%, a major road is flooded, and students have exams approaching. The system must respond with safe routes, feasible allocations, clear priorities, honest shortage reporting, and continuously updated decisions.

**Protect the existing ROUTELESS project. Make the emergency response real. Put students' safety and examination access first.**

**GramRoute AI — No Student Left Stranded.**
